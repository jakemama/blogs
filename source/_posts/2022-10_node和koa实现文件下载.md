---
title: node和koa实现文件下载
tags:
  - 笔记
categories:
  - 前端
date: 2022-10-21 16:28:06
---


<!--more-->
# 简单下载
最简单的情况就是服务器上文件系统已经存在了某个文件，客户端请求下载直接把文件读了吐回去即可：

```js
import Koa from 'koa';
import Router from 'koa-router';
import * as fs from 'fs/promises';

const app = new Koa();
const router = new Router();

router.get('/download/simple', async (ctx) => {
  const file = await fs.readFile(`${__dirname}/1.txt`, 'utf-8');
  ctx.set({
    'Content-Disposition': `attachment; filename=1.txt`,
  });
  ctx.body = file;
});

app.use(router.routes());
app.listen(80);
```

> 设置 Content-Disposition 头部为 attachment 是关键，告诉浏览器应该下载这个文件。

# 流式下载
简单下载在碰到大文件的情景就不够用了，因为 Node 无法将大文件一次性读取到进程内存里。这时候用流来解决：

```js
router.get('/download/stream', async (ctx) => {
  const file = fs.createReadStream(`${__dirname}/1.txt`);
  ctx.set({
    'Content-Disposition': `attachment; filename=1.txt`,
  });
  ctx.body = file;
});
```

> 此例子不设置 Content-Disposition 头部也是会下载的，因为 Content-Type 被设置为了 application/octet-stream，浏览器认为其是一个二进制流文件所以默认下载处理了。

# 进度显示

当下载的文件特别大时，上个例子 Content-Length 正确设置时浏览器下载条里就能正常显示进度了，为了方便我们使用程序模拟一下：

```js
router.get('/download/progress', async (ctx) => {
  const { enable } = ctx.query;
  const buffer = await fsp.readFile(`${__dirname}/1.txt`);
  const stream = new PassThrough();
  const l = buffer.length;
  const count = 4;
  const size = Math.floor(l / count);
  const writeQuarter = (i = 0) => {
    const start = i * size;
    const end = i === count - 1 ? l : (i + 1) * size;
    stream.write(buffer.slice(start, end));

    if (end === l) {
      stream.end();
    } else {
      setTimeout(() => writeQuarter(i + 1), 3000);
    }
  };

  if (!!enable) {
    ctx.set({
      'Content-Length': `${l}`,
    });
  }

  ctx.set({
    'Content-Type': 'plain/txt',
    'Content-Disposition': `attachment; filename=1.txt`,
    Connection: 'keep-alive',
  });
  ctx.body = stream;
  writeQuarter();
});
```

> 这里利用了 PassThrough 流来替代 fs.createReadStream，故 Koa 不再知道文件大小和类型，并将文件分为 4 份，每份间隔 3 秒发送来模拟大文件下载。
> 当参数 enable 为真时，设置了 Content-Length 则会显示进度 (剩余时间），否则不显示

# 断点续传

下载文件特别大时，常常也会因为网络不稳定导致下载中途断开而失败，这时候可以考虑支持断点续传：

```js
function getStartPos(range = '') {
  var startPos = 0;
  if (typeof range === 'string') {
    var matches = /^bytes=([0-9]+)-$/.exec(range);
    if (matches) {
      startPos = Number(matches[1]);
    }
  }
  return startPos;
}

router.get('/download/partial', async (ctx) => {
  const range = ctx.get('range');
  const start = getStartPos(range);
  const stat = await fsp.stat(`${__dirname}/1.txt`);
  const stream = fs.createReadStream(`${__dirname}/1.txt`, {
    start,
    highWaterMark: Math.ceil((stat.size - start) / 4),
  });

  stream.on('data', (chunk) => {
    console.log(`Readed ${chunk.length} bytes of data.`);
    stream.pause();
    setTimeout(() => {
      stream.resume();
    }, 3000);
  });

  console.log(`Start Pos: ${start}.`);
  if (start === 0) {
    ctx.status = 200;
    ctx.set({
      'Accept-Ranges': 'bytes',
      'Content-Length': `${stat.size}`,
    });
  } else {
    ctx.status = 206;
    ctx.set({
      'Content-Range': `bytes ${start}-${stat.size - 1}/${stat.size}`,
    });
  }

  ctx.set({
    'Content-Type': 'application/octet-stream',
    'Content-Disposition': `attachment; filename=1.txt`,
    Connection: 'keep-alive',
  });
  ctx.body = stream;
});
```

> （Chrome 默认下载工具不支持断点续传）
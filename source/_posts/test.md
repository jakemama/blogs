---
title: 第一篇文章
date: 2022-03-25 14:19:35
tags:
---
# 第一篇文章
第一篇文章

``` javascript
function replaceTable(node) {
  let tableList = Array.from(node.getElementsByTagName("table"));Array.from(node.getElementsByTagName("table"));Array.from(node.getElementsByTagName("table"));
  if (tableList.length === 0) return;
  tableList.forEach((table) => {
    // if (table.querySelectorAll("img").length == 0) {
    //     return;
    // }
    let div1 = document.createElement("div");
    let bigdiv = document.createElement("div");
    bigdiv.setAttribute("cms-style", "scroll");
    div1.setAttribute("cms-style", "maxWidth");
    let clonetable = table.cloneNode(true);
    div1.appendChild(clonetable);
    bigdiv.appendChild(div1);
    table.parentNode.insertBefore(bigdiv, table);
    table.parentNode.removeChild(table);
  });
}
```
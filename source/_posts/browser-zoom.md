---
title: 網頁縮放程式碼 Browser Zoom
date: 2022-10-24 22:12:39
categories: Javascript
tags: [Source Code]
---

<br/>

```javascript
// zoomRate: 縮放倍數, 1 代表原本尺寸
function zoom(zoomRate) {
    
    var percent = zoomRate * 100;

    // 設定 zoom, width
    document.body.style.position = "relative";
    document.body.style.zoom = percent + "%";
    document.body.style.width = percent + "%";
    document.body.style.overflow = "visible";

    // window dispatch resize 事件, 讓有綁 resize 的元件做 rwd 縮放
    var resizeEvent = window.document.createEvent('UIEvents'); 
    resizeEvent.initUIEvent('resize', true, false, window, 0); 
    window.dispatchEvent(resizeEvent);
}
```
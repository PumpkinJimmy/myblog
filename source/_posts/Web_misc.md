---
date: 2021-10-31 11:10:27
title: 前端杂项
---
# 前端杂项
- `Element.getBoundingClientRect`以及`Element.getClientRects`可以用于获得计算后的大小。因为很多时候页面大小都是相对单位（比如说`em, vh, vw`之类的），直接查询DOM是得不到实际大小的。偏偏事件里的信息都是绝对定位的。
- `window.onresize`可以用于响应式，因为viewport变化的时候就会触发`onresize`
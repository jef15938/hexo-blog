---
title: 效能測試 Performance Test
date: 2022-11-03 14:30:56
categories: Performance
tags: [Tool Introduce]
---

<div style="width:128px; height:128px">

![](lighthouse.png "lightHouse")

</div>

## LightHouse

一個開源工具, 用來檢測 performance, accessibility, progressive web apps, SEO and more.

### 運行方式
1. 直接由 chrome dev tools 執行 [Run Lighthouse in Chrome DevTools](https://developer.chrome.com/docs/lighthouse/overview/#devtools)
2. 使用 node command line 執行 [Install and run the Node command line tool](https://developer.chrome.com/docs/lighthouse/overview/#cli)
3. 使用 chrome extension 執行 [Run Lighthouse as a Chrome Extension](https://developer.chrome.com/docs/lighthouse/overview/#extension)

### 查看報告
<div style="max-width: 600px">

![](lighthouse-report.png)

</div>

上方區塊會顯示個項目的分數, 這邊 focus 在 Performance
Performance 分數會由 6 個項目決定, 比重請參考 [lighthouse 計分方式](https://web.dev/performance-scoring/?utm_source=lighthouse&utm_medium=devtools)


## [First Contentful Paint (FCP)](https://web.dev/first-contentful-paint/?utm_source=lighthouse&utm_medium=unknown)

當使用者進入網站, 瀏覽器花費多少時間渲染第一個 DOM 內容(images, non-white canvas, svgs 都算, 但是 iframe 裡的任何東西都不算)

## [Speed Index](https://web.dev/speed-index/?utm_source=lighthouse&utm_medium=devtools)

畫面從開始載入到載入完成, 計算中間的過程, 是否"平滑", 可以參考[這篇](https://ypwalter.blogspot.com/2018/06/perceptual-speed-index.html)解釋

## [Largest Contentful Paint (LCP)](https://web.dev/lighthouse-largest-contentful-paint/?utm_source=lighthouse&utm_medium=devtools)

使用者打開網頁後，在當前 viewport 看到的最大物件被呈現出來要多少時間

## [Time to Interactive (TTI)](https://web.dev/interactive/?utm_source=lighthouse&utm_medium=devtools)

瀏覽器花多久時間達到"完整互動"狀態, 達到"完整互動"要滿足以下條件
* The page displays useful content, which is measured by the First Contentful Paint,
* Event handlers are registered for most visible page elements, and
* The page responds to user interactions within 50 milliseconds.

那實際上怎麼計算(可以參考[這裡](https://web.dev/tti/))
1. 從 FCP 開始.
2. 搜尋出現 quiet window 至少 5 秒, 並記下該時間 ( quiet window 定義: 沒有 Long Task 且當下正在執行的 Get requests 不多於 2 個.
[Long Task API](https://developer.mozilla.org/en-US/docs/Web/API/Long_Tasks_API)
    * 長時間的 running event handlers.
    * 複雜的 reflows and other re-renders.
    * event loop 執行時間 that 超過 50 ms.
3. 從第二步拿到的時間往前搜尋, 找到最後的 Long Task 結束時間
4. TTI 就是最後的 Long Task 結束時間 (如果都沒有 Long Task, 那 TTI 就是 FCP).


The following diagram should help visualize the steps above:

![](calculate.svg)

## [Total Blocking Time (TBT)](https://web.dev/lighthouse-total-blocking-time/?utm_source=lighthouse&utm_medium=devtools#what-tbt-measures)

TBT 為一個總和的時間, 當瀏覽器凍結畫面 from responding to user input, such as mouse clicks, screen taps, or keyboard presses 都會加總進去
TBT 的數值為 FCP 到 TTI 之間的 "long tasks" blocking portion 時間總和, "long tasks" 代表對於 user input 的反應超過 50ms 的行為。假設偵測到一個 70ms 的 long tasks, 那它的 blocking portion = 70ms - 50ms = 20ms

## [Cumulative Layout Shift (CLS)](https://web.dev/cls/?utm_source=lighthouse&utm_medium=devtools)

CLS 為最大的 layout shift scores
layout shift scores = impact fraction * distance fraction

以下呈現 CLS 對使用者造成的影響


![](cls-demo.gif)

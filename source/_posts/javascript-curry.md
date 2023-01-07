---
title: javascript 柯里化
date: 2023-01-07 10:23:51
categories: Functional Javascript
tags: [Source Code]
---

<br/>

## 柯里化
<blockquote>
可以理解為提前接收部分參數，延遲執行，不立即輸出結果，而是返回一個接受剩餘參數的函數。因為這樣的特性，也被稱為部分計算函數。
</blockquote>

<br/>

```javascript
// 數字加總柯里化
function sum(x) {
    // 第一個參數都沒傳, 回傳 0
    if (x === undefined) {
        return 0;
    }
    let total = x;
    return function sumInside(y) {
        // 設立 function 執行終點, 無傳入參數的話, 回傳加總結果
        if (y === undefined) {
            return total;
        } 
        // 有傳入參數的話, 將參數加到 total, 並回傳"接收一個參數的 function"(柯里化的核心)
        else {
            total += y;
            return sumInside;
        }
    }
}
sum() // 0
sum(100)() // 100
sum(100)(30)() // 130
sum(100)(30)(-50)() // 80
```
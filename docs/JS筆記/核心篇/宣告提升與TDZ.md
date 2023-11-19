---
# SEO
title: 宣告提升與TDZ暫時性死區
description: 宣告提升與TDZ暫時性死區
# image: 在社交媒体卡片中显示的缩略图
keywords: [宣告提升, hoisting, TDZ, 暫時性死區]
sidebar_position: 2
---

# 宣告提升與 TDZ

## 提升 hoisting 是什麼？

這裡的用字：聲明=宣告。

它指的是『在程式碼执行之前』，變數和函式聲明/宣告會被移動到『它們所在作用域的顶部』。
雖然这个過程稱為`提升`，但實際上並不是將變數或函式的定義真正提升到程式碼的顶部，而是`在编譯階段將它们的聲明/宣告提升`。

概括的介紹一下提升的概念：

```js
var a = 1;
console.log(a);
```

把上面程式碼拆解為，會有兩個階段：

### 1.創造階段 (建立記憶體空間，當變數與函式命名衝突時，函式優先！)

```js
var a; //undefined
```

### 2.執行階段 (賦予值)

```js
a = 1;
console.log(a);
```

## 變數聲明提升

### var 的變數聲明 提升

當我們使用 var 宣告變數時，這個變數的聲明會被提升到當前作用域的頂部，
這意味著我們可以`在變數聲明之前訪問該變數`，但其值會是 undefined（不會報錯）：

```js
console.log(myVar); // 輸出 undefined
var myVar = 42;
```

因為上面程式碼實際執行時，是這樣：

```js
var myVar; // 變數聲明被提升
console.log(myVar); // 輸出 undefined
myVar = 42; // 賦值操作保持在原位置
```

加上若是在全域環境用 var 聲明變數，該變數也同時會是全域 window 下的屬性。

### let const 的變數聲明 提升 -> TDZ 暫時性死區

使用 let 和 const 宣告的變數也會被提升，`但它們在變數聲明之前是不可訪問的，會導致暫時性死區（Temporal Dead Zone，TDZ）`。在 TDZ 中，如果嘗試訪問 let 或 const 變數，會導致 ReferenceError 錯誤。

```js
console.log(myVar); // 這裡會報 ReferenceError 錯誤
let myVar = 42;
```

實際執行時，上述的程式碼並不會像 var 一樣將變數聲明提升到頂部，`而是保留在區塊作用域的範圍內。在區塊作用域外部，該變數是不可訪問的，直到執行它的聲明行`。

#### 暫時性死區（Temporal Dead Zone，TDZ）

TDZ 是 JavaScript 中的一個特性，它發生在使用 let 和 const 宣告變數時。TDZ 表示變數在宣告（declaration）之後，但在初始化（assignment）之前的這一段期間，無法被訪問或引用。

```js
console.log(myVar); // 這裡會報 ReferenceError 錯誤
let myVar = 42;
```

myVar 在宣告之前就被訪問，因此導致 TDZ 錯誤。正確的方式是將變數宣告和初始化放在同一行

##### 1.提升：變數宣告被提升

使用 let 和 const 宣告的變數和 var 宣告的變數一樣都會被提升到它們所在的作用域，
`但不像 var 那樣被初始化(賦值)為 undefined`。
let 和 const 宣告的變數在 TDZ 內，程式碼還沒有讀到"let 和 const 宣告變數的那行"，該值是不可以被訪問的，任何嘗試訪問它們都會導致 ReferenceError。

##### 2.TDZ 的範圍

TDZ 的範圍僅限於 let const 變數宣告所在的區塊（block）作用域。當變數在區塊作用域內宣告時，TDZ 僅在該區塊作用域內有效。

#### 3.TDZ:初始化後(賦值之後)才可訪問

要使 let const 宣告的變數`變為可訪問`，必須在宣告之後立即初始化(賦值)變數，否則它將一直處於 TDZ 狀態，直到初始化(賦值)為止。

#### TDZ 總結：

暫時性死區的引入是為了解決在 var 中可能出現的變數提升和初始化問題。它有助於提高代碼的安全性，確保變數在使用之前必須先初始化，從而減少了代碼中的潛在錯誤。總之，使用 let 和 const 來聲明變數時，需要特別注意變數的初始化順序，以避免暫時性死區錯誤

## 函式宣告提升

函式宣告提升就是函式宣告會在程式碼執行之前被提升，這表示你可以在函式宣告之前呼叫該函式，而不會出現錯誤。

```js
drink(); // 可以在函式宣告之前呼叫函式
sleep();

function drink() {
  console.log("喝咖啡");
}

function sleep() {
  console.log("睡覺");
}
```

實際執行時，程式碼是這樣：

```js
//函式宣告提升到當前作用域的頂部
function drink() {
  console.log("喝咖啡");
}

//函式宣告提升到當前作用域的頂部
function sleep() {
  console.log("睡覺");
}

drink();
sleep();
```

### 函式宣告是可以重複的，後面的宣告會覆蓋前面的宣告。

```js
function sleep() {
  console.log("睡覺");
}

function sleep() {
  //後面的函式宣告sleep會覆蓋前面的函式宣告sleep。
  console.log("睡不了");
}

sleep(); //輸出"睡不了"
```

## 練習：

### 例子 1： 創造階段與執行階段

寫出創造階段與執行階段的順序

```js
console.log(b);
var b = 2;
console.log(b);
```

正解：<br/> 1.創造階段

```js
var b; //建立記憶體空間
```

2.執行階段（執行&賦值）

```js
console.log(b); //undefined
b = 2;
console.log(b); //2
```

### 例子 2： 創造階段與執行階段

寫出創造階段與執行階段的順序

```js
var a = 1;

function a() {
  console.log("123");
}

console.log(a);
```

正解如下： <br/> 1.創造階段

```js
function a() {
  console.log("123");
} //創造階段，創建的記憶體空間-> 函式優先！

var a; //創造階段，再來才是變數
```

2.執行階段（執行&賦值）

```js
a = 1;
console.log(a); //以最後創建空間為主,輸出1
```

<hr/>

這題如果順序顛倒一下，結果很不同喔！

```js
console.log(a);
var a = 1;

function a() {
  console.log("123");
}
```

正解如下： <br/> 1.創造階段

```js
function a() {
  console.log("123");
} //創造階段，創建的記憶體空間-> 函式優先！

var a; //創造階段，再來才是變數
```

2.執行階段（執行&賦值）

```js
console.log(a); //fn a() {console.log("123");}
a = 1;
```

這題的 a 會是函式 a;

## undefined 與 not defined 的差異

```js
console.log(a); //這裡會出現 a is not defined的錯誤
var b;
console.log(b); //undefined
b = 3;
```

a is not defined 是一個錯誤，一定要修正，不然 js(程式碼逐行執行觀念)程式碼無法執行
undefined 不是報錯，因此後續程式碼仍可以執行

以物件 undefined 舉例：

```js
const obj = {};
console.log(obj.name); //物件取第一層屬性，不存在只會回傳undefined
console.log(obj.name.firstClass); //試圖存取物件第二層屬性，會報錯，
//"TypeError: Cannot read properties of undefined (reading 'f')
```

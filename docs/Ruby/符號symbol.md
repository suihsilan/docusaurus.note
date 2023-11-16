---
# SEO
title: 符號 symbol
description: 符號 symbol
# image: 在社交媒体卡片中显示的缩略图
keywords: [符號 symbol, Ruby]
sidebar_position: 7
---

# 符號 symbol

還記得 ruby 的 Hash 舊式寫法嗎？

```ruby
profile = { :name => 'kk', :age => 18}
```

這裡的冒號，就是符號

## 符號『不是變數』，它就是一個值，也是『有名字的物件』

:::note

> 1 是一個數字物件 <br />
> "aa"是一個字串物件 <br />
> :hello 是一個符號物件 <br />

:::

區分一下變數(標籤)與符號(值/有名字的物件)的差別：

```ruby
p :hello  # 輸出 :hello
p 2  # 輸出 2
p "aa"  # 輸出 "aa"

name = "kk"   # 而name才是變數

# 錯誤寫法 因為符號就是值也是有名字的物件 符號不是變數！
:hello = 2
2 = "kk"
```

```ruby
my_name = "喬巴" # my_name 變數指向/被賦值 一個字串
your_name = :someone # your_name 變數指向/被賦值 符號
```

## 字串 與 符號的差別

:::note
字串的內容可以變，但 Symbol 不行
:::

### 字串的內容可以變

```ruby
name = "abcde"

name[1] = "x"

p name   #輸出結果是 "axcde"
```

字串的內容被改掉了！

### Symbol 符號的內容是不能更動的

```ruby
name = :abcde

name[1] = "x"

p name   #會報錯 undefined method `[]=' for :abcde:Symbol (NoMethodError)
```

### 字串的效能差一點點

在 ruby 的世界裡，你每一個物件都會有一個共有的方法就是 <br />

> .object_id

他會印出該物件在 ruby 中的序號(記憶體位置)=> 每次執行都不同 <br />
因為每次執行時它都是不一樣的物件 因為執行完就還掉了

但若是我今天如果重複執行的是同一個 symbol

```ruby
p "hello".object_id #同一個字串每次序號不同
p "hello".object_id #同一個字串每次序號不同
p "hello".object_id #同一個字串每次序號不同

p :hello.object_id #同一個符號每次序號相同
p :hello.object_id #同一個符號每次序號相同
p :hello.object_id #同一個符號每次序號相同
```

### 現在知道記憶體序號=> 就來驗證為什麼『數字物件』是符號

在 ruby 中數字物件的序號是 2n+1，數字物件的記憶體位置是相同的，因此數字是符號

```ruby
p 2.object_id # 5
p 5.object_id # 11
p 11.object_id # 23
p 11.object_id # 23
```

## 冷凍字串 => 可讓某一字串的序號冷凍起來(序號就不會變)

當你冷凍字串之後，等於說記憶體位置就不變了，會具有符號特質=> 冷凍字串不能被更改

```ruby
p "hello".freeze.object_id #印出740
p "hello".freeze.object_id #印出740
p "hello".freeze.object_id #印出740
```

## 字串與符號互相轉換

### 字串 轉符號 .to_sym

```ruby
p "hello".to_sym #印出 符號 :hello
p "hello".intern #印出 符號 :hello
```

### 符號 轉字串 .to_s

```ruby
p :hello.to_s #印出 字串 "hello"
p :hello.id2name #印出 字串 "hello"
```

複習 .to_a 就是轉成陣列

## 該使用 字串 還是符號？

不可變 ＝> 選擇 符號 or 冷凍字串
可變 ＝> 選擇 字串

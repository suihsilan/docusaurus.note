---
# SEO
title: 雜湊 hash
description: 雜湊 hash
# image: 在社交媒体卡片中显示的缩略图
keywords: [雜湊 hash, Ruby]
sidebar_position: 6
---

# 雜湊 hash {key:value}

```ruby
# 使用 Hash 類別
a = Hash.new

# 使用大括號
b = {}
```

也就是可以這樣建立 Hash

```ruby
profile = { name: 'kk', age: 18}
p profile

# 輸出時長這樣：
{:name=>"kk", :age=>18}
```

## 在 ruby 中，hash 有兩種寫法：

### 舊式 hash 寫法

```ruby
profile = { :name => 'kk', :age => 18}
```

### 新式 hash 寫法 ruby1.9 之後(像 JSON 格式)

其實新式只是語法糖，和前述本質沒有變

```ruby
profile = { name: 'kk', age: 18}
```

## 取出 Hash 的 key 與 values

```ruby
profile = { name: 'kk', age: 18}

p profile.keys  # 回傳[:name, :age]
p profile.values # 回傳["kk", 18]
```

## 要用對的 key 才能拿到對的 value(回歸舊式 Hash 本質)

js 寫習慣，容易在 ruby 這裡犯錯：

```ruby
profile = { name: 'kk', age: 18}

p profile["name"] # 這樣的寫法是錯誤的 在ruby中是nil

# 要用對的key => ruby的hash本質
p profile[:name] # 才會輸出"kk"
```

了解更多https://ruby-doc.org/3.2.2/Hash.html

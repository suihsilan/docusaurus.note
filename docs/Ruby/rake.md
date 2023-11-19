---
# SEO
title: Rake任務
description: Rake任務
# image: 在社交媒体卡片中显示的缩略图
keywords: [Rake任務, Ruby]
sidebar_position: 10
---

# Rake 任務

先認識 make，<br />
在軟體開發中，Make 是一個工具程式，會讀取叫做<br />
『makefile』的檔案，根據 makefile 的內容去自動化的建構與編譯軟體<br />

在 Ruby 中，也有類似的工具叫做 Rake

## Rake = Ruby 版的 Make

是由 Jim Weirich 所開發的，Make 有 makefile<br />
Rake 則是 rakefile/Rakefile

```bash
$rake

# 回傳
rake aborted!
No Rakefile found
```

rake 程式是在我們下載 ruby 時就已經包含在裡面了<br />
當我鍵入指令 rake 卻回傳找不到，<br />
是因為我還沒有建立 rakefile 檔案<br />
先建立 rakefile 檔案，並開始在裡面寫入與定義任務

```bash
$touch rakefile
```

### 什麼是 Rake?

在 chatgpt 中解釋：
:::note
Rake 是 Ruby 的一種建構工具，用於定義、描述和執行任務。它是一種類似於 Make、Ant 或 Maven 的工具，<br />
但是針對 Ruby 語言進行了優化。Rake 的任務是由 Ruby 語言撰寫的，這使得它相對靈活和容易使用。<br />

Rake 主要用於自動化程式建構、測試、部署以及其他常見的開發任務。<br />
Rakefile 是 Rake 使用的配置檔案，其中包含了任務的定義和它們之間的依賴關係。<br />

Rake 的一個特點是它使用 Ruby 語法，這意味著你可以在 Rakefile 中使用 Ruby 的強大功能來處理複雜的任務邏輯。<br />
此外，Rake 支援命令行參數，使你可以根據需要動態地配置任務。<br />

以下是一些 Rake 的基本概念：<br />

- 任務（Task）： 在 Rakefile 中定義的一個工作或操作。例如，執行測試、建構應用程序等都可以是一個任務。<br />

- 依賴關係（Dependency）： 一個任務可能依賴於其他一個或多個任務。這種依賴關係確保在執行某個任務之前，它所依賴的任務會被執行。<br />

- Rakefile： 包含任務定義的主要配置檔案，通常是一個名為 Rakefile 或 rakefile.rb 的檔案。<br />

- 命令行工具： 通過命令行執行 Rakefile 中定義的任務。例如，使用 rake test 可以執行測試任務。<br />
  總的來說，Rake 是 Ruby 社群中常用的自動化工具之一，尤其在 Ruby on Rails 專案中被廣泛使用。
  :::

### 舉例：

假設我們有一個 Ruby 專案，我們想要使用 Rake 來執行測試、建構應用程式和清理臨時檔案。<br />

```rake
# 測試任務
task :test do
  puts "啟動專案"
  # 在這裡加入執行測試的命令
  # 例如：`bundle exec rspec` 或 `ruby test/test_file.rb`
end

# 建構任務
task :build => :test do
  puts  "正在構建..."
   # 在這裡加入建構的相關指令
   # 例如：`bundle exec jekyll build` 或 `rake assets:precompile`
end

# 清理臨時檔案任務
task :clean do
  puts "清理暫存檔案..."
  # 在這裡加入清理臨時檔案的指令
  # 例如：`rm -rf tmp/*` 或 `bundle exec rake tmp:clear`
end
```

在這個範例中，我們定義了三個任務：:test、:build 和 :clean。<br />
:build 任務依賴於 :test，這表示在執行 :build 之前會先執行 :test。<br />
這種依賴結構確保了執行順序，從而確保在建構之前先執行測試。<br />

可以在終端機中執行這些任務，例如：

```bash
# 執行測試
rake test

# 執行建構（會先執行測試）
rake build

# 清理臨時檔案
rake clean
```

這只是一個簡單的示例，實際上你的 Rakefile 可能會包含更多的任務，依賴於你的專案需求。<br />你可以根據自己的需求擴展這個 Rakefile，並使用 Ruby 的強大功能來自動化你的開發流程。

## rakefile 檔案 => 假設現在要自己編寫任務

```rake
# 編寫任務=>寄送email
task :sendmail do
  puts "get email list from database"
  sleep 2
  puts "sending email..."
  sleep
  puts "done"
end
```

回到終端機中，輸入上面相關指令，即可執行該任務

```bash
$ rake sendmail
```

在每個任務最上面加上一個描述 desc "mail sender"

```rake
desc "mail sender"
# 編寫任務=>寄送email
task :sendmail do
  puts "get email list from database"
  sleep 2
  puts "sending email..."
  sleep 2
  puts "done"
end
```

若在 bash 指令輸入兩個指令擇一

```bash
$ rake -T

$ rake --tasks
```

回傳

```bash
rake sendmail  # mail sender
```

再繼續輸入指令

```bash
$ rake sendmail
```

就會開始跑你編寫的 task :sendmail 流程

```bash
get email list from database
sending email...
done
```

### 可以編寫預設的 task

```rake
desc "mail sender"
# 編寫任務=>寄送email
task :sendmail do
  puts "get email list from database"
  sleep 2
  puts "sending email..."
  sleep 2
  puts "done"
end

# 編些預設的任務 讓它指向:sendmail任務
task :default => :sendmail
```

這樣一來，當你在終端機輸入預設 rake 指令：

```bash
$ rake
```

就會去幫你做預設的任務

## 有些任務會有相依性

執行 A 任務前，要先執行 B 任務<br />
上廁所前要先開門：

```rake
task :open_the_door do
  puts "open the door開門!"
end

task :goto_toilet => :open_the_door do
  put "上廁所!"
end
```

```rake
$ rake goto_toilet
# open the door開門!
# 上廁所
```

## rails db:migrate

其實這個原本就是 <br />

> $ rake db:migrate

```rakefile
task :migrate do
  puts "migration!!!"
end
```

```rake
#輸入
$ rake db:migrate

# 卻回傳找不到
```

所以就要回到 rakefile 裡，加上 namespace

```rakefile
namespace :db do
  task :migrate do
  puts "migration!!!"
  end
end

```

### 如果要做 $ rails db:migrate

Rake 任務

```rakefile
namespace :db do
  desc '資料庫處理'
  task :migrate do
    puts "migration!!!"
  end
end
```

### 為什麼要加上 namespace?

gpt 回答：

在 Ruby on Rails 中，$ rake db:migrate 這樣的指令通常使用了 Rake 的 namespace 功能。<br />
namespace 用於將任務組織成一個層次結構，以避免命名衝突和更好地組織你的任務。<br />

在 db:migrate 的例子中，db 是一個命名空間，它將所有與資料庫相關的任務組織在一起。這樣做的好處有：<br />

1.組織性： 將相關的任務分組在一個命名空間下，使得 Rakefile 更易讀和維護。資料庫相關的任務（如建立、遷移、種子資料等）都可以放在 db 命名空間下。<br />

2.避免命名衝突： 如果你的專案中有多個任務，而其中一些可能有相同的名稱，使用 namespace 可以避免這些命名衝突。<br />
在 db 命名空間下，你可以有 migrate、seed 等相關的子任務，而不會與其他命名空間下的相同名稱的子任務發生衝突。<br />

示例：<br />

```rakefile
# Rakefile

namespace :db do
  desc "Run migrations"
  task :migrate do
    # 實際執行遷移的指令
    puts "Running database migrations..."
  end

  desc "Seed the database"
  task :seed do
    # 實際執行種子資料的指令
    puts "Seeding the database..."
  end
end
```

在這個例子中，db:migrate 和 db:seed 分別是 db 命名空間下的兩個任務。<br />
這樣，你可以避免命名衝突，並且更容易理解和管理資料庫相關的任務。<br />
當你執行 $ rake db:migrate 時，Rake 會知道要執行的是 db 命名空間下的 migrate 任務。<br />

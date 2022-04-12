# Ruby on Rails

## 1 簡介

### 1.1 Ruby on Rails?

**Ruby on Rails**(官方簡稱為Rails，RoR非官方簡稱)是使用Ruby程式語言所開發的Web框架。Rails採用MVC(Model-View-Control)模式、內建支援單元測試與整合測試、支援Ajax和RESTful介面、ORM機制，以及支援各種最新的業界標準如HTML5、JQuery等等功能。

Rails的哲學包括以下指導原則：

* **不要重複自己(DRY: Don’t Repeat Yourself)** – 撰寫出重複的程式碼是件壞事。
* **慣例勝於設定(Convention Over Configuration)** – Rails會預設各種好的設定跟慣例，而不是要求你設定每一個細節到設定檔中。
* **REST是網站應用程式的最佳模式** – 使用Resources和標準的HTTP verbs(動詞)來組織你的應用程式是最快的方式(我們會在路徑一章詳細介紹這個強大的設計)。

### 1.2 Ruby

Ruby是一套開放原始碼、物件導向的動態直譯式(interpreted)程式語言，它有著簡單哲學、高生產力、精巧、自然的語法。創造者是來自日本的松本行弘(又名Matz)，設計的靈感來自於Lisp、Perl和Smalltalk，設計的目的是要讓程式設計師能夠快樂地寫程式。

範例：

```ruby
str = "Ruby Test"
5.times { puts str }
```

以上的範例能知道有關Ruby的三件事：

* 動態分型(typing)，無須宣告型態
* 每樣東西都是物件，包括數字
* 使用Code Block形式的匿名函式(anonymous function)隨處可見

### 1.3 RubyGems

RubyGems是Ruby的套件管理系統，讓你輕易安裝及管理Ruby函式庫。你可以在[RubyGems](https://rubygems.org/)上找到所有的Ruby開源套件。另外，讀者如果想找Ruby或Rails有哪些好用的套件，也可以瀏覽看看[The Ruby Toolbox](https://www.ruby-toolbox.com/)，這個站依照套件的熱門程度排序，非常方便。

#### 常用指令

```bash
gem -v RubyGems的版本
gem update --system 升級RubyGems的版本
gem install <gem_name> 安裝某個套件
gem list 列出安裝的套件
gem update <gem_name> 更新最新版本
gem update 更新所有安裝的Gems
gem install -v x.x.x <gem_name> 安裝特定版本
gem uninstall <gem_name> 反安裝
```

執行`gem install <gem_name>`時，安裝完會自動產生此套件的RDoc和ri文件。但只要Google或是在套件官網就可以查詢到文件，不太需要在本地端機器產生文件，安裝的時間耗時又佔硬碟空間。要省略這個步驟，可以輸入下方指令：

安裝時輸入：

```bash
gem install <gem_name> -N
```

## 2 安裝 Ruby

### 2.1 作業系統

Ruby可以運行在Windows、Linux、Mac OS X、BSD和Solaris上。雖然Rails可以在Windows上執行，但是有些套件只支援Unix-like作業系統，以及Ruby程式在Unix-like系統上執行起來也比較快速及穩定。**所以這裡我選擇使用`WSL2運行Ubuntu 20.04LTS`來當作我的開發環境**。

### 2.2 資料庫

Rails支援的資料庫包括SQLite3、MySQL、Postgres、IBM DB2、Oracle和SQL Server等。除了安裝資料庫軟體，我們也需要安裝搭配的Ruby函式庫(稱作Adapter或Driver)。作為新手的單機練習，使用`SQLit`就可以了。

### 2.3 開始安裝(環境為WSL2)

首先要先安裝Ruby需要的項目

```bash
sudo apt update
sudo apt install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev
```

下一步要安裝管理Ruby版本的管理器`rbenv`。安裝`rbenv`需要安裝`rbenv`和`ruby-build`

```bash
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL
```

之後在rbenv上安裝Ruby，可以下載自己所需要的版本

```bash
rbenv install -l
rbenv install 3.1.1
rbenv global 3.1.1
```

檢查Ruby的版本

```bash
ruby -v
```

最後在安裝`Bundler`

```bash
gem install bundler
```

## 3 安裝 Rails

使用以下指令安裝 Rails

```bash
gem install rails -v 7.0.2.3
```

如果有使用`rbenv`，需要使用以下指令套用

```bash
rbenv rehash
```

檢查 Rails是否安裝

```bash
rails -v
# Rails 7.0.2.3
```

之後將WSL2關閉再重開即可

```bash
wsl --shutdown
```

## 4 建立一個Blog Application

在你預先建立好要存放rails的地方，輸入下面指令建立一個叫做`blog`的Rails專案：

```bash
rails new blog
```

輸入後可以看到以下訊息新增的檔案

```bash
create
create  README.md
create  Rakefile
create  .ruby-version
create  config.ru
create  .gitignore
create  .gitattributes
create  Gemfile
...
Bundle complete! 15 Gemfile dependencies, 73 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
```

進入剛剛建立的blog資料夾

```bash
cd blog
```

這邊簡單的介紹各個檔案的功用：

| 檔案/目錄     | 用途    |
|--------------|:--------------|
| Gemfile  | 設定Rails應用程式會使用哪些Gems套件 |
| README   | 專案說明，告訴他人這個應用程式的功用 |
| Rakefile | 載入可以被命令列執行的一些Rake任務 |
| app/     | 放Controllers、Models和Views檔案，接下來的內容主要都在這個目錄 |
| config/  | 應用程式設定檔、路由規則、資料庫設定等等 |
| config.ru| 用來啟動應用程式的Rack伺服器設定檔 |
| db/      | 資料庫的結構綱要 |
| doc/     | 用來放你的文件 |
| lib/     | 放一些自定的Module和類別檔案 |
| log/     | 應用程式的Log記錄檔 |
| public/  | 唯一可以在網路上看到的目錄，圖檔、JavaScript、CSS和其他靜態檔案擺放的地方 |
| bin/     | 放rails這個指令和放其他的script指令 |
| test/    | 單元測試、fixtures及整合測試等程式 |
| tmp/     | 暫時性的檔案 |
| vendor/  | 用來放第三方程式碼外掛的目錄 |

## 5 啟動伺服器

Rails使用了一套叫做`Bundler`的工具可以幫助我們檢查及安裝這個Rails應用程式所有依存的套件，在blog資料夾下輸入：

```bash
bundle install
```

> 也可以只輸入bundle。每次有修改Gemfile就要在重新執行。

輸入以下指令，啟動伺服器

```bash
bin/rails server
```

> rails server 可以簡寫為 rails s

之後會出現以下訊息：

```text
=> Booting Puma
=> Rails 7.0.2.3 application starting in development
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.6.4 (ruby 3.1.1-p18) ("Birdie's Version")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 179
* Listening on http://127.0.0.1:3000
* Listening on http://[::1]:3000
Use Ctrl-C to stop
```

打開瀏覽器前往[http://localhost:3000](http://localhost:3000)，我們可以看到Rails的預設首頁。

## 6 Hello, Rails!

要讓Rails說"你好"，要先建立一個`route`、`controller`、`view`。route會先映射請求到control，control會處理請求所需要的工作，並準備任何view所需要的資料，view會顯示這些資料。

Ruby如何實現這些工作：
* Routes 是由Ruby DSL (Domain-Specific Language)規則所編寫。
* Controllers 是 Ruby classes，公共的函式式`actions`。
* Views 通常為 HTML與Ruby的混合。

首先，我們先加入route，到`config/routes.rb`加入程式碼：

```ruby
Rails.application.routes.draw do
  get "/articles", to: "articles#index"
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html
end
```

這段宣告表示route為 `GET /articles`，而 request會映射到`ArticlesController`裡面的`index` action。

這時候我們還沒有`ArticlesController`這個 controller，可以使用 controller產生器來產生檔案(`--skip-routes`因為我們已經先產生了route，所以skip)：

```bash
bin/rails generate controller Articles index --skip-routes
```

> rails generate 可以簡寫為 rails g

Rails 會幫你產生數個檔案：

```text
create  app/controllers/articles_controller.rb
invoke  erb
create    app/views/articles
create    app/views/articles/index.html.erb
invoke  test_unit
create    test/controllers/articles_controller_test.rb
invoke  helper
create    app/helpers/articles_helper.rb
invoke    test_unit
```

產生的controller檔案會在，`app/controllers/articles_controller.rb`

```ruby
class ArticlesController < ApplicationController
  def index
  end
end
```

當`index` action是空的時候，Rails會自動顯示與controller action名稱相同的view。Views的位置在`app/views`，你可以找到與之action同名的`app/views/articles/index.html.erb`。

更改裡面的內容為：

```html
<h1>Hello, Rails!</h1>
```

再啟動伺服器，到 [http://localhost:3000/articles](http://localhost:3000/articles)就可以看到變化了。

目前 [http://localhost:3000](http://localhost:3000)還是顯示 Rails預設的畫面，我們可以把自己做的歡迎畫面導到主頁。新增一個 route並把 root path映射到相對的controller。

開啟`config/routes.rb`，新增 `root` route：

```ruby
Rails.application.routes.draw do
  root "articles#index"

  get "/articles", to: "articles#index"
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html
end
```

再到 [http://localhost:3000](http://localhost:3000)觀看變化。


## 7 Generating a Model

根據 `MVC (Model-View-Controller)`模式，我們已經完成`controller`與`view`，接下來就是 `model`。

使用 Ruby產生器產生model：

```bash
bin/rails generate model Article title:string body:text
```

Rails 會幫你產生數個檔案：

```bash
invoke  active_record
create    db/migrate/20220411145014_create_articles.rb
create    app/models/article.rb
invoke    test_unit
create      test/models/article_test.rb
create      test/fixtures/articles.yml
```

這裡要注意兩個檔案：

1. `db/migrate/20220411145014_create_articles.rb`
2. `test/fixtures/articles.yml`

### 7.1 Database Migrations

遷移 (Migrations)用於更改資料庫的結構。在 Rails Application中，遷移是用 Ruby編寫的。

migration file：

```ruby
class CreateArticles < ActiveRecord::Migration[7.0]
  def change
    create_table :articles do |t|
      t.string :title
      t.text :body

      t.timestamps
    end
  end
end
```

調用`create_table`指定 `articles`該如何建造table。默認情況下，`create_table`函式會新增一個自動遞增的主鑑(auto-incrementing primary key)，名為 `id`。

我們也在一開始的時候，定義了兩個 columns: `title` and `body`。

最後，`t.timestamps` 這個方法定義了另外兩個名為 `create_at`與 `update_at`。Rails會幫我們管理創建與更新模型時當下的時間。

使用下列的指令做`遷移`：

```bash
bin/rails db:migrate
```

Output：

```text
== 20220411145014 CreateArticles: migrating ===================================
-- create_table(:articles)
   -> 0.0087s
== 20220411145014 CreateArticles: migrated (0.0088s) ==========================
```

這樣我們就建立 table並使用了。

### 7.2 使用 Model與 Database溝通

Rails有一個功能可以與 Database溝通，稱為 `console`。Console是一個相似於互動式編碼的 `irb`。

使用下列指令運行 console，並可以看到 `irb` prompt：

```bash
bin/rails console

Loading development environment (Rails 7.0.2.3)
irb(main):001:0> 
```

在該 prompt下，我們可以新增一個 `Article`物件：

```bash
article = Article.new(title: "Hello Rails", body: "I am on Rails!")
```

此時我們新增一個 `Article`物件，但這個物件現在還沒有儲存進資料庫裡，他目前只存在於這個 console上，要把該物件寫入資料庫，必須使用 `save`指令：

```bash
article.save

#TRANSACTION (0.1ms)  begin transaction
#Article Create (4.0ms)  INSERT INTO "articles" ("title", "body", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "Hello Rails"], ["body", "I am Rails!"], ["created_at", "2022-04-12 05:23:21.990518"], ["updated_at", "2022-04-12 05:23:21.990518"]]
#TRANSACTION (637.2ms)  commit transaction                         
=> true
```

上面可以看到 articles執行了 SQL語法 `INSERT INTO "articles" ...`，如果這時候在使用 `article`物件一次，就會出現：

```bash
article
=> 
#<Article:0x00007f5ca4fd58d8
id: 1,                    
title: "Hello Rails",     
body: "I am Rails!",      
created_at: Tue, 12 Apr 2022 05:18:31.460794000 UTC +00:00,
updated_at: Tue, 12 Apr 2022 05:18:31.460794000 UTC +00:00>
```

`id`, `created_at`, and `updated_at`都被設定好了，這也說明了 Rails確實的幫我們儲存在這些物件。

如果要在資料庫裡找到 article，可以使用 `find`：

```bash
Article.find(1)
#<Article:0x00007f5ca4ac7238      
 id:1,                                                
 title: "Hello Rails",                                             
 body: "I am Rails!",                                             
 created_at: Tue, 12 Apr 2022 05:18:31.460794000 UTC +00:00,       
 updated_at: Tue, 12 Apr 2022 05:18:31.460794000 UTC +00:00>  
```

輸出所有 articles，可以使用 `all`：

```bash
Article.all
[#<Article:0x00007f5ca5699e80  
  id:1,                       
  title: "Hello Rails",                                           
  body: "I am Rails!",                                          
  created_at: Tue, 12 Apr 2022 05:18:31.460794000 UTC +00:00,      
  updated_at: Tue, 12 Apr 2022 05:18:31.460794000 UTC +00:00>]
```

### 7.3 在 Views上顯示 Articles

我們知道如何使用 model與資料庫溝通，現在我們可以獲取資料庫的資料，將資料傳給 views讓它呈現。

首先，先到 controller `app/controllers/articles_controller.rb`，並更改 `index` action：

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
end
```

根據上面，我們從資料庫獲得所有的 Articles，並且指定到 `@articles`。

接著到 `app/views/articles/index.html.erb`加入：

```erb
<h1>Hello, Rails!</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
	  <%= article.title %>
	</li>
  <% end %>
</ul>
```

上方程式碼是HTML與ERB的混合。`ERB`是一種模板系統 (Templating system)，用於嵌入 Ruby程式碼。這裡有兩種 ERB標籤：

* `<% %>` 在範圍裡執行 ruby code。
* `<%= %>` 輸出返回的值到 ERB。

再到 [http://localhost:3000](http://localhost:3000)，可以看到所有 Article的名稱。

## Source

[Rails Guides](https://guides.rubyonrails.org/getting_started.html)

[Rails 實戰聖經](https://ihower.tw/rails/index.html)

[Install Ruby On Rails on Windows 10 (WSL2)](https://gorails.com/setup/windows/10)
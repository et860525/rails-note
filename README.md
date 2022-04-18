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

## 8 CRUD

接下來要開始設計 `CRUD (Create、Read、Update、Delete)`操作。

### 8.1 顯示單一 Article (Read)

首先，我們要先加入新的 route，開啟 `config/routes.rb`：

```ruby
Rails.application.routes.draw do
  root "articles#index"
  
  get "/articles", to: "articles#index"
  get "/articles/:id", to: "articles#show"
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html
end
```

`:id`：這是 route parameter (路由參數)。route parameter會獲得所指定部份的請求路徑，把它放進`params`。使用 controller可以獲得params。

例如：請求 `GET http://localhost:3000/articles/1`，`1`會成為 `:id`的值，可以在 `ArticlesController`裡使用 `params[:id]`獲得該值。

我們在 `app/controllers/articles_controller.rb`新增一個新的 action名為 `show`：

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
  
  def show
    @article = Article.find(params[:id])
  end
end
```

之後再創立新的 view，`app/views/articles/show.html.erb`：

```erb
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>
```

再到 [http://localhost:3000/articles/1](http://localhost:3000/articles/1)，就可以看到文章了。

### 8.2 Resourceful Routing

現在我們已經建立 CRUD裡面的 Read了，還有其他的 Create、 Update、Delete需要建立，當然也要新增與之相對的 controller action與 views。每當有`Routes`、`Controller`、`Views`這三個組合執行 CRUD操作，這都稱為 `resource`。

Rails提供給 routes一個名為 `resources`方法，他有所有 CRUD需要的 routes，我們可以把 `config/routes.rb`所新增的兩個 get route改換成 `resources`：

```ruby
Rails.application.routes.draw do
  root "articles#index"

  resources :articles
end
```

使用 `bin/rails routes` 指令可以檢查所有的 routes：

```bash
bin/rails routes
      Prefix Verb   URI Pattern                  Controller#Action
        root GET    /                            articles#index
    articles GET    /articles(.:format)          articles#index
 new_article GET    /articles/new(.:format)      articles#new
     article GET    /articles/:id(.:format)      articles#show
             POST   /articles(.:format)          articles#create
edit_article GET    /articles/:id/edit(.:format) articles#edit
             PATCH  /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
```

觀看上面可以發現，不只幫我們設定好 routes，連controller action都設定好了。

`resources`還設計了 URL、path helper methods，我們可以使用後綴 (Prefix)：`_url`或`_path` 來回傳 URL。例如：當要連結到一篇文章時，`article_path`會回傳 `"/articles/#{article.id}"`，可以用它來連接我們 `index.html.erb`與 `show.html.erb`：

```erb
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <a href="<%= article_path(article) %>">
        <%= article.title %>
      </a>
    </li>
  <% end %>
</ul>
```

這裡一個方法叫做 `link_to`，`link_to`會把指定的對象轉換成路徑。例如：我們要從`index`進到一篇`article`裡，`link_to`會調用 `article_path`：

```erb
<h1>Hello, Rails!</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <%= link_to article.title, article %>
    </li>
  <% end %>
</ul>
```

`link_to`的第一個參數為連接的文字，第二個參數是傳送一個model。

### 8.3 建立一個新的 Article (Create)

繼續建立CRUD裡面的 `Create`。在Web裡，建立新的資源需要經過許多步驟。用戶必須先填寫表單，然後再提交。如果沒有任何錯誤就保存進資料庫。如果有錯誤，就重新顯示表單並加上錯誤訊息，以此反覆。

在 Rails裡，這些步驟都由 controller `new`和 `create`處理，在 `app/controllers/articles_controller.rb`加入以下程式碼：

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
  
  def show
    @article = Article.find(params[:id])
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(title="...", body="...")
    
    if @article.save
      redirect_to @article
    else
      render :new, status: :unprocessable_entity
    end
  end
end
```

* `new` action：實例化一篇新文章，但並不會執行保存。這個實例是傳給 view做表單用的。`new`會把該實例傳給 `app/views/articles/new.html.erb`。

* `create` action：使用 title和 body的值實例化一篇新文章，並嘗試保存它。如果文章保存成功，瀏覽器會導向 `http://localhost:3000/articles/#{@article.id}`。如果失敗，會由 `app/views/articles/new.html.erb`呈現 狀態代碼 `422 Unprocessable Entity`來重新顯示表單。目前 title與 body都是暫時的值，等一下會做改動。

> `redirect_to` 會讓瀏覽器做一個新的 request。`render`則是指定一個 view並傳送當下的 request。簡單來說，如果使用 `render`做保存後的顯示，`render`會無法得到值，因為 request還是舊的。

#### 8.3.1 使用表單產生器 (Form Builder)

這裡使用Rails裡的一個功能名為`表單產生器 (Form Builder)`，`form builder`可以使用最少的程式碼，來產生表單。

新建檔案 `app/views/articles/new.html.erb`並加入：

```erb
<h1>New Article</h1>

<%= form_with model: @article do |form| %>
  <div>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```

使用 `form_with` helper method實例化表單產生器。在`form_with`區塊裡，我們可以使用像是 `label`和 `text_field`的這些方法，來建立我們所需要的表單。

使用 `form_with`產生的結果：

```html
<form action="/articles" accept-charset="UTF-8" method="post">
  <input type="hidden" name="authenticity_token" value="...">

  <div>
    <label for="article_title">Title</label><br>
    <input type="text" name="article[title]" id="article_title">
  </div>

  <div>
    <label for="article_body">Body</label><br>
    <textarea name="article[body]" id="article_body"></textarea>
  </div>

  <div>
    <input type="submit" name="commit" value="Create Article" data-disable-with="Create Article">
  </div>
</form>
```

#### 8.3.2 使用強參數 (Strong Parameters)

提交表單後，提交的資料與捕捉到的 route參數都會被放在名為 `params`的 Hash裡。`create` action可以通過訪問已經提交的 `params[:article][:title]`和 `params[:article][:body]`。但是，當隨著添加更多表單，這些字段會變得冗長並且容易出錯。

所以我們可以傳入一個單一的 Hash。但是我們必須要指定哪些資料可以放進這個 Hash，否則，惡意用戶可能會提交額外的表單欄位來更改private的資料。其實如果你直接傳送沒過濾的 `params[:article]` Hash至 `Article.new`。Rails會出現 `ForbiddenAttributesError`警告。為了過濾 `params`，我們使用 Rails裡的 **Strong Parameters**來過濾 `params`。

新增一個 private方法名為 `article_params`到 `app/controllers/articles_controller.rb`，並加到 `create` action：

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)

    if @article.save
      redirect_to @article
    else
      render :new, status: :unprocessable_entity
    end
  end

  private
    def article_params
      params.require(:article).permit(:title, :body)
    end
end
```

#### 8.3.3 驗證與顯示錯誤訊息

建立新資源要經過許多步驟，處理無效的用戶輸入也是其中之一。Rails提供一個名為 `validations`的方法幫助我們檢查這些資料。Validations 是在 model object被保存前的一個規則檢查機制，如果有任何錯誤，保存將會被中止，並且會回傳相應 model object屬性的錯誤訊息。

在 `app/models/article.rb`新增 `validations`到我們的 model：

```ruby
class Article < ApplicationRecord
  validates :title, presence: true
  validates :body, presence: true, length: { minimum: 10 }
end
```

* 第一個 validation：`presence: true`表示 `title`值必須存在。而因為 `title`是 string型別，所以 `title`的值必須要包含至少一個非空白字元。

* 第二個 validation：與上面的一樣，`body`值必須存在。`length: { minimum: 10 }`這表示 `body`值必須最少要有10個字元。

> `title`和 `body`的這些屬性都是由 `Active Record`自動生成的，它會為每個 column自動定義這些屬性。

接下來我們要放置 validations，讓我們修改 `app/views/articles/new.html.erb`來顯示關於 `title`和 `body`的錯誤訊息：

```erb
<h1>New Article</h1>

<%= form_with model: @article do |form| %>
  <div>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
    <% @article.errors.full_messages_for(:title).each do |message| %>
      <div><%= message %></div>
    <% end %>
  </div>

  <div>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
    <% @article.errors.full_messages_for(:body).each do |message| %>
      <div><%= message %></div>
    <% end %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```

`full_messages_for`方法會回傳一組array為指定屬性的錯誤訊息。如果沒有任何的錯誤，則array會是空白的。

#### 8.3.4 步驟解釋與結尾

最後來整理一下 `new` action和 `create` action是如何工作的，先來看下方程式碼：

```ruby
def new
  @article = Article.new
end

def create
  @article = Article.new(article_params)

  if @article.save
    redirect_to @article
  else
    render :new, status: :unprocessable_entity
  end
end
```

當訪問 [http://localhost:3000/articles/new](http://localhost:3000/articles/new)時，`GET /articles/new` request會映射 `new` action。`new` action不會嘗試保存 `@article`，而 validations也不會檢查，不會產生任何的錯誤訊息。

當提交表單後，`POST /articles` request會映射 `create` action。`create` action會嘗試保存 `@article`並且 validations會做檢查。如果驗證失敗，`@article`不會被保存，並回傳加上錯誤訊息的`app/views/articles/new.html.erb`。

現在已經完成了 `Create`步驟了，把[http://localhost:3000/articles/new](http://localhost:3000/articles/new)連結加到 `app/views/articles/index.html.erb`：

```erb
<h1>Hello, Rails!</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <%= link_to article.title, article %>
    </li>
  <% end %>
</ul>

<%= link_to "New Article", new_article_path %>
```

### 8.4 更新 Article (Update)

接下來繼續完成 "U" (Update)。Update與 Create十分相似，它們都有多個步驟要處理。首先，使用者 request一個表單來修改資料。提交表單後，如果沒有任何錯誤，資料就會被更新，否則，就會顯示錯誤訊息，依此反覆。

Update由 controller的 `edit` action與 `update` action來實作。新增這兩個 action到 `app/controllers/articles_controller.rb`：

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
  
  def show
    @article = Article.find(params[:id])
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)
    
    if @article.save
      redirect_to @article
    else
      render :new, status: :unprocessable_entity
    end
  end

  def edit
    @article = Article.find(params[:id])
  end

  def update
    @article = Article.find(params[:id])

    if @article.update(article_params)
      redirect_to @article
    else
      render :edit, status: :unprocessable_entity
    end
  end

  private
    def article_params
      params.require(:article).permit(:title, :body)
    end
end
```

有發覺 `edit`, `update` actions 與 `new`, `create` actions有多相像了吧。

首先，`edit` action會獲取資料庫裡指定的 article，並將它儲存在 `@article`能讓我們製作表單。默認情況下，`edit` action render的 view是 `app/views/articles/edit.html.erb`。

`update` action會重新在資料庫獲取一次指定的資料，並在表單提交後，嘗試用已過濾的 `article_params`更新它。如果沒有任何  `validations`失敗，則會 redirect到該文章的畫面，否則會回傳 `app/views/articles/edit.html.erb`並帶著錯誤訊息。

#### 8.4.1 共享部分相同的程式碼

這裡可以注意到，不只 controller相似，連輸出的 form也是與 `new` action一樣的。多虧了 Rails的 `form builder`與 `resourceful route`，form builder 會根據 model object來自動變化表單的內容。

由於程式碼一樣，我們可以把程式碼分解到單獨的一個 view裡，讓程式碼可以達到共用的功能。建立 `app/views/articles/_form.html.erb`：

```erb
<%= form_with model: article do |form| %>
  <div>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
    <% article.errors.full_messages_for(:title).each do |message| %>
      <div><%= message %></div>
    <% end %>
  </div>

  <div>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
    <% article.errors.full_messages_for(:body).each do |message| %>
      <div><%= message %></div>
    <% end %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```

上面的程式碼相同於 `app/views/articles/new.html.erb`，只是所有的 `@article`全部都被改成 `article`。這是因為 partials是共享程式碼，他們不依賴於 controller設定的特定 variable。相反的，我們將從 article傳送 local variable。

讓我們更新 `app/views/articles/new.html.erb`套用 partial的設定，並傳入 article：

```erb
<h1>New Article</h1>

<%= render "form", article: @article %>
```

> partial檔案名稱前方必須帶有底線，例如：_form.html.erb。但是在 render時，底線必須要拿掉，例如：render "form"。

建立 `app/views/articles/edit.html.erb`：

```erb
<h1>Edit Article</h1>

<%= render "form", article: @article %>
```

#### 8.4.2 結尾

現在我們可以修改文章了，將修改連結放到每篇文章上。到 `app/views/articles/show.html.erb`修改程式碼：

```erb
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>

<ul>
  <li><%= link_to "Edit", edit_article_path(@article) %></li>
</ul>
```

### 8.5 刪除 Article (Delete)

最後到了 "D" (Delete)。`Delete`相比 Create和 Update簡單許多。它只需要一個 route與一個 controller action。而我們的  resourceful routing已經為我們設定了。`DELETE /article/:id`映射 request到 `ArticlesController`的 `destroy` action。

新增 `destroy` action到 `app/controllers/articles_controller.rb`：

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
  
  def show
    @article = Article.find(params[:id])
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)
    
    if @article.save
      redirect_to @article
    else
      render :new, status: :unprocessable_entity
    end
  end

  def edit
    @article = Article.find(params[:id])
  end

  def update
    @article = Article.find(params[:id])

    if @article.update(article_params)
      redirect_to @article
    else
      render :edit, status: :unprocessable_entity
    end
  end

  def destroy
    @article = Article.find(params[:id])
    @article.destroy

    redirect_to root_path, status: :see_other
  end

  private
    def article_params
      params.require(:article).permit(:title, :body)
    end
end
```

`destroy` action會從資料庫裡獲取指定的資料，並呼叫 `destroy`刪除它。成功後，瀏覽器會被 redirect到根目錄 (root path)下，status code為 `303 See Other`。

最後把 Delete連結放到各個文章下 `app/views/articles/show.html.erb`：

```erb
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>

<ul>
  <li><%= link_to "Edit", edit_article_path(@article) %></li>
  <li><%= link_to "Destroy", article_path(@article), data: {
                    turbo_method: :delete,
                    turbo_confirm: "Are you sure?"
                  } %></li>
</ul>
```

根據上面的程式碼，我們使用 `data` option來設定 "Destroy"連結的 `data-turbo-method`和 `data-turbo-confirm`屬性。這兩個屬性都來自 Rails內建的應用程式 [Turbo](https://turbo.hotwired.dev/)。
當連結被觸發時：

* `data-turbo-method="delete"`：會丟出 `DELETE` request替代原本的 `GET` requet。
* `data-turbo-confirm="Are you sure?"`： 會跳出確認視窗，如果使用者按下取消，request會被終止。

到這裡，已經完成了 CRUD了。

## 9 新增第二個 Model

### 9.1 產生 Model

接下來新增一個新名為 `Comment`的 Model。這裡也使用與產生 `Article` Model時所使用的產生器：

```bash
bin/rails g model Comment commenter:string body:text article:references
```

指令會產生4個檔案：

|檔案|目的|
|----------|-----------|
|db/migrate/20220415061358_create_comments.rb|Migration會在資料庫裡建立 comment table|
|app/models/comment.rb| Comment model|
|test/models/comment_test.rb| 測試 comment model|
|test/fixtures/comments.yml| 產生測試 comment model的範例|

首先，先看到 `app/models/comment.rb`：

```ruby
class Comment < ApplicationRecord
  belongs_to :article
end
```

與 `Article` model的一開始非常像，唯一不同的就是多了一段 `belongs_to :article`。它設定 Comment是 Active Record `association`與 Article，這也就是接下來的重點。

在 shell設定 model時，我們使用了 (`:references`) keyword，而它接受的型別就是 model。它會在你的資料庫的 table上建立一行新的 column，裡面存放的資料是該 model的名稱加上 `_id`的整數 (Integer)，用以下的例子會比較清楚。

到剛剛建立的 `db/migrate/20220415061358_create_comments.rb`：

```ruby
class CreateComments < ActiveRecord::Migration[7.0]
  def change
    create_table :comments do |t|
      t.string :commenter
      t.text :body
      t.references :article, null: false, foreign_key: true

      t.timestamps
    end
  end
end
```

`t.references`建立一個整數 column名為  `article_id`，由 foreign key來指向 `articles`的 `id` column，接著使用 migration來讓資料庫套用這些設定：

```bash
bin/rails db:migrate
```

Rails只會執行還沒有 migrations的部分，依據回傳的狀態你能看到：

```bash
== 20220415061358 CreateComments: migrating ===================================
-- create_table(:comments)
   -> 0.0077s
== 20220415061358 CreateComments: migrated (0.0078s) ==========================
```

### 9.2 關聯 (Associating) Model

Active Record associations能讓你簡單的宣告兩個 models之間的關係 (relationship)。而 comments與 articles之間的關係為 `One-To-Many`：

* 每一條 comments都屬於一篇 article

* 一篇 article可以有多個 comments

事實上，association的宣告與 Rails的語法非常相似，而且我們也早就已經看到了。就是 `Comment` moddel (app/models/comment.rb)它讓每個 comment都屬於 Article：

```ruby
class Comment < ApplicationRecord
  belongs_to :article
end
```

現在編輯 `app/models/article.rb`來新增另外一邊的 association：

```ruby
class Article < ApplicationRecord
  has_many :comments

  validates :title, presence: true
  validates :body, presence: true, length: { minimum: 10 }
end
```

這樣一來，你也能使用 `@article.comments`來獲得 comment的資料。

### 9.3 新增一個 Comments Route

與 articles controller一樣，我們需要新增一個 route，讓 Rails知道我們想要如何導到 `comments`。打開 `config/routes.rb`並編輯：

```ruby
Rails.application.routes.draw do
  root "articles#index"
  
  resources :articles do
    resources :comments
  end
end
```

在 `articles`裡新增 一個 nested resource 的`comments`。這是另一部分捕捉 article和 comments之間存在的層次關係。

其關係只要使用 `bin/rails routes`就能看的到：

```text
              Prefix Verb   URI Pattern                                             Controller#Action
                root GET    /                                                       articles#index
    article_comments GET    /articles/:article_id/comments(.:format)                comments#index
                     POST   /articles/:article_id/comments(.:format)                comments#create
 new_article_comment GET    /articles/:article_id/comments/new(.:format)            comments#new
edit_article_comment GET    /articles/:article_id/comments/:id/edit(.:format)       comments#edit
     article_comment GET    /articles/:article_id/comments/:id(.:format)            comments#show
                     PATCH  /articles/:article_id/comments/:id(.:format)            comments#update
                     PUT    /articles/:article_id/comments/:id(.:format)            comments#update
                     DELETE /articles/:article_id/comments/:id(.:format)            comments#destroy
```

### 9.4 產生 Controller

產生完 model後，現在就要產生 controller：

```bash
bin/rails g controller Comments
```

會建立4個檔案與1個空資料夾：

| 檔案/資料夾 | 目的 |
|-----------|-------------|
|app/controllers/comments_controller.rb | Comments controller |
|app/views/comments/ | Comments views |
|test/controllers/comments_controller_test.rb | 測試 controller |
|app/helpers/comments_helper.rb | view helper檔案 |

Blog能讓讀者在閱讀文章後直接留下他們的評論，一但增加評論後，將會回到原本的文章並顯示剛剛留下的評論。使用 `CommentsController`建立新增評論和刪除評論的功能。

首先，將修改顯示文章的 template (app/views/articles/show.html.erb)將它改成能發表新評論：

```erb
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>

<ul>
  <li><%= link_to "Edit", edit_article_path(@article) %></li>
  <li><%= link_to "Destroy", article_path(@article), data: {
                    turbo_method: :delete,
                    turbo_confirm: "Are you sure?"
                  } %></li>
</ul>

<h2>Add a Comment:</h2>
<%= form_with model: [ @article, @article.comments.build ] do |form| %>
  <p>
    <%= form.label :commenter %><br>
    <%= form.text_field :commenter %>
  </p>
  <p>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </p>
  <p>
    <%= form.submit %>
  </p>
<% end %>
```

以上程式碼會在顯示 `Article`的頁面上加入一個表單，該表單可以調用 `CommentsController`的 `create` action來建立新的評論。這裡調用 `form_with`使用一組 array，它會建構一個 nested route，像是 `/articles/1/comments`。

接著在 `app/controllers/comments_controller.rb`建立 `create`：

```ruby
class CommentsController < ApplicationController
  def create
    @article = Article.find(params[:article_id])
    @comment = @article.comments.create(comment_params)
    redirect_to article_path(@article)
  end

  private
    def comment_params
   params.require(:comment).permit(:commenter, :body)
  end
end
```

這個 controller的程式碼可能會比 article的更加複雜一點。這是因為我們設定了 nesting所造成的。每個評論的 request都必須跟著該評論所附加的文章，因此最一開始使用 `find`方法來獲取 `Article`。

此外，程式碼會利用一些 association的方法。`@article.comments`使用 `create`方法來保存這些評論。它會自動連接評論，並讓它屬於特定文章。

新增完評論後，Controller就會導向一開始的文章 `article_path(@article)`。而我們希望評論會顯示在各自的文章裡，所以將它新增到
 `app/views/articles/show.html.erb`：

```erb
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>

<ul>
  <li><%= link_to "Edit", edit_article_path(@article) %></li>
  <li><%= link_to "Destroy", article_path(@article), data: {
                    turbo_method: :delete,
                    turbo_confirm: "Are you sure?"
                  } %></li>
</ul>

<h2>Comments</h2>
<% @article.comments.each do |comment| %>
  <p>
    <strong>Commenter:</strong>
    <%= comment.commenter %>
  </p>
  <p>
    <strong>Comment:</strong>
    <%= comment.body %>
  </p>
<% end %>

<h2>Add a Comment:</h2>
<%= form_with model: [ @article, @article.comments.build ] do |form| %>
  <p>
    <%= form.label :commenter %><br>
    <%= form.text_field :commenter %>
  </p>
  <p>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </p>
  <p>
    <%= form.submit %>
  </p>
<% end %>
```

這樣就完成了整個留言系統。

## 10 重構 (Refactoring)

`8.4.1`節使用的共享程式碼的方式。這裡即將使用此方式來簡短 `app/views/articles/show.html.erb`。

### 10.1 Rendering Collections

首先，顯示 Comments的部分，我們可以建立一個新的檔案名為 `app/views/comments/_comment.html.erb`來存放以下程式碼：

```erb
<p>
  <strong>Commenter:</strong>
  <%= comment.commenter %>
</p>
<p>
  <strong>Comment:</strong>
  <%= comment.body %>
</p>
```

並更改 `app/views/articles/show.html.erb`：

```erb
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>

<ul>
  <li><%= link_to "Edit", edit_article_path(@article) %></li>
  <li><%= link_to "Destroy", article_path(@article), data: {
                    turbo_method: :delete,
                    turbo_confirm: "Are you sure?"
                  } %></li>
</ul>

<h2>Comments</h2>
<%= render @article.comments %>

<h2>Add a Comment:</h2>
<%= form_with model: [ @article, @article.comments.build ] do |form| %>
  <p>
    <%= form.label :commenter %><br>
    <%= form.text_field :commenter %>
  </p>
  <p>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </p>
  <p>
    <%= form.submit %>
  </p>
<% end %>
```

`app/views/comments/_comment.html.erb`會顯示每個 comment的內容。`render` 會對`@article.comments` 使用each，並把每個 comment rendering。在這個情況下，comment就是 local variable的方式呈現。

### 10.2 Rendering Form

將新增 comment的部分也把它做成一個檔案，`app/views/comments/_form.html.erb`並把原本的 form移動到該檔案：

```erb
<%= form_with model: [ @article, @article.comments.build ] do |form| %>
  <p>
    <%= form.label :commenter %><br>
    <%= form.text_field :commenter %>
  </p>
  <p>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </p>
  <p>
    <%= form.submit %>
  </p>
<% end %>
```

編輯 `app/views/articles/show.html.erb`：

```erb
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>

<ul>
  <li><%= link_to "Edit", edit_article_path(@article) %></li>
  <li><%= link_to "Destroy", article_path(@article), data: {
                    turbo_method: :delete,
                    turbo_confirm: "Are you sure?"
                  } %></li>
</ul>

<h2>Comments</h2>
<%= render @article.comments %>

<h2>Add a comment:</h2>
<%= render 'comments/form' %>
```

第二個 render只 render特定的 template，`comments/form`。Rails很聰明，使用`/`就可以獲得 `app/views/comments`裡面的 `_form.html.erb`(記得在 render裡不需要"`_`")。

## Source

[Rails Guides](https://guides.rubyonrails.org/getting_started.html)

[Rails 實戰聖經](https://ihower.tw/rails/index.html)

[Install Ruby On Rails on Windows 10 (WSL2)](https://gorails.com/setup/windows/10)

# [小学生でも分かるRuby on Rails入門](https://openbook4.me/projects/92/)

### アプリスケルトンの生成

```
# この段階では枠(フォルダ)だけ作っている。
$ rails new アプリ名
$ rails new mitakalab-twitter
```

### コントローラーとビューを生成

```
$ rails g controller コントローラー名 ビュー名
$ rails g controller users index show
```

### サービス開始

```
$ rails s
```

### エラー

```
# ビューで表示したい情報がない
undefined method `[]' for nil:NilClass
```

アクセスの流れ：

```
ユーザーがURLにアクセス ⇒ ルーティングが仕分け ⇒ コントローラーが値を入れる ⇒ ビューがコントローラーから渡された値を表示
```

ルーティング

```ruby
MitakalabTwitter::Application.routes.draw do
  get "users/index"
  # "users/show/:username"の意味：URLごとに変わる値はコロンをつけている
  # "users#show"の意味：usersコントローラのshowに行け
  get "users/show/:username" => "users#show"
  ...
end
```

***

### データベースの考え方

```
データベース : 1万ページの白いノート
スキーマ　　 : 情報を入力するための共通の枠
```

データベース作成コマンド

```
# 真っ白なノートを買ってきました。
$ rake db:create
```

マイグレーションファイル作成コマンド

```
# 枠組み、つまり真っ白ノートに貼り付けたいテンプレート
$ rails g model user name:string username:string location:string about:text
```

<注意点>

> モデルはusersではなく、userという単数型である。
>
> これはrailsのルール

マイグレートするコマンド

```
# テンプレートを真っ白ノートに貼り付ける作業
$ rake db:migrate
```

db/seeds.rb

```
#初期データを入れるためのファイル
@モデル名 = モデル名(１文字目は大文字).new
...
@モデル名.save
--------------------------------------
# 例：
@user = User.new
...
@user.save
```

初期データをDBに書き込む

```
$ rake db:seed
```

***

### DBのデータを表示する

DBのデータを表示するにはコントローラをいじる

```
# DBからユーザー名を指定して情報を取得する
@user = User.find_by(:username => params[:username])
```

[find_byについて]([https://qiita.com/mr-myself/items/cfb936dcf63d2c44d2f5](https://qiita.com/mr-myself/items/cfb936dcf63d2c44d2f5))

***

### ツイートを保存するためのビュー、コントローラー、モデルを作る

1. ビューとコントローラーを作る

```
# new が新しいツイートを入力する画面用
$ rails g controller tweets index show new
```

2. モデルを作る(マイグレーションファイル作成)

   ```
   $ rails g model tweet title:string content:text
   ```

3. DBに反映(マイグレート)

   ```
   $ rake db:migrate
   ```

4. newのビューを作る

   Railsではカンタンにフォームを作成できる。

   注 このようなものをヘルパーと言います。 この例の場合は、フォームヘルパーと呼ばれるものです。

   ```erb
   <%= form_for Tweet.new do |f| %>
     <%= f.label :title %>
     <%= f.text_field :title %>
     <%= f.label :content %>
     <%= f.text_area :content %>
     <%= f.submit %>
   <% end %>
   ```

5. ルーティング設定

   ```ruby
   # config/routes.rb
   MitakalabTwitter::Application.routes.draw do
     get "tweets/index"
     get "tweets/show"
     get "tweets/new"
     # "tweets#create":tweetsコントローラのcreateに行け！
     # ここで、ボタンを押したから"post"を使ったかな？
     post "tweets" => "tweets#create"
   end
   ```

6. createアクションの定義

   ```ruby
   class TweetsController < ApplicationController
     def index
     end
   
     def show
     end
   
     def new
     end
   
     def create
       # ユーザーから送られてきた情報をDBに保存する
       @tweet = Tweet.new
       @tweet.title = params[:tweet][:title]
       @tweet.content = params[:tweet][:content]
       @tweet.save
       # データを保存したら一覧にリダイレクトする
       redirect_to '/tweets/index'
     end
   end
   ```

7. ツイートの一覧表示

   7.1 コントローラ部分

   ```ruby
   class TweetsController < ApplicationController
     def index
       # DBからすべてのデータを取ってくる
       @tweets = Tweet.all
     end
     ...
   end
   ```

   7.2 ビュー部分

   ```erb
   <% @tweets.each do |tweet| %>
     <h1><%= tweet.title %></h1>
     <p><%= tweet.content %></p>
   <% end %
   ```

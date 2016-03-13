---
layout: post
title:  "PhoenixFramework を使ってミニブログを作る"
date:   2016-03-13 17:00:00 +0900
categories: phoenix
---
# アプリの雛形を作成

~~~
$ mix phoenix.new miniblog
$ mix phoenix.server
~~~

* http://localhost:4000 で起動を確認できる

# Ecto 経由で DB を作成

~~~
$ mix ecto.create
~~~

* config/dev.exs に記載された database の名前で作成される
* psql コマンドで確認する

~~~
$ psql -U postgres
# \l (データベース一覧表示)
~~~

# Article の CRUD を作成

~~~
$ mix phoenix.gen.html Article articles title:string description:string
~~~

* web/router.ex に下記のように resource を追加

~~~
resources "/articles", ArticleController
~~~

* migration を実施

~~~
$ mix ecto.migrate
~~~

* miniblog_dev DB に articles テーブルが作成されたことを確認



# router の確認

~~~
$ mix phoenix.routes
   page_path  GET     /                   Miniblog.PageController :index
article_path  GET     /articles           Miniblog.ArticleController :index
article_path  GET     /articles/:id/edit  Miniblog.ArticleController :edit
article_path  GET     /articles/new       Miniblog.ArticleController :new
article_path  GET     /articles/:id       Miniblog.ArticleController :show
article_path  POST    /articles           Miniblog.ArticleController :create
article_path  PATCH   /articles/:id       Miniblog.ArticleController :update
              PUT     /articles/:id       Miniblog.ArticleController :update
article_path  DELETE  /articles/:id       Miniblog.ArticleController :delete
~~~

* サーバを起動して articles を確認
    * http://localhost:4000/articles


# 動作確認

* 一覧表示
    * URL
        * http://localhost:4000/articles
    * routes
        * index

* 記事を新規で作成
    * URL
        * http://localhost:4000/articles/new
    * routes
        * new -> create


* 詳細表示
    * URL
        * http://localhost:4000/articles/1
    * routes
        * show


* 既存記事を更新
    * URL
        * http://localhost:4000/articles/1/edit
    * routes
        * edit -> update


* 既存記事を削除
    * URL : なし
    * routes
        * delete

---
layout: post
title:  "CentOS で Elixir/Phoenix 環境を構築する"
date:   2016-03-12 22:00:00 +0900
categories: elixir
---
# 前提
* Vagrant で CentOS を用意し、その上で環境構築をする

~~~
$ vagrant --version
~~~

Vagrant 1.8.1

# Vagrant Box の用意

~~~
$ vagrant init centos/7
~~~

* Vagrantfile の private_network 設定をする
    * コメントアウトを外すだけ

~~~
$ vagrant up
~~~

# パッケージインストール

## vim, git, screen

~~~
$ sudo yum update
$ sudo yum install -y vim git screen postgresql-server
~~~

## gcc 他
~~~
$ sudo yum install gcc glibc-devel make ncurses-devel openssl-devel autoconf
~~~

## kerl & Erlang
~~~
$ curl -O https://raw.githubusercontent.com/yrashk/kerl/master/kerl
$ chmod a+x kerl
~~~

* PATH の通っている場所に移動
    * 今回は ~/bin/ に kerl を移動

* 公開されているバージョン一覧を表示

~~~
$ kerl list releases
~~~

* 一覧の更新

~~~
$ kerl update releases
~~~

* 18.2.1 をビルド

~~~
$ kerl build 18.2.1 18-2-1_default_config
~~~

* ビルドしたバージョン一覧に追加されていることを確認

~~~
$ kerl list builds
~~~

* インストール済みの一覧を表示
    * まだインストールされていない

~~~
$ kerl list installations
~~~

* 18.2.1 をインストール

~~~
$ kerl install 18-2-1_default_config ~/bin/erlang/18-2-1_default_config
~~~

* インストール済みの一覧を表示

~~~
$ kerl list installations
~~~

* アクティベート

~~~
$ . bin/erlang/18-2-1_default_config/activate
$ erl でアクティベートされていることを確認
~~~

## Elixir
* ディレクトリ指定

~~~
$ cd ~/bin/
~~~

* git cloen

~~~
$ git clone https://github.com/elixir-lang/elixir.git
~~~

* v1.2.3 tag にスイッチ

~~~
$ git tag
$ git checkout v1.2.3
(最新のタグにチェックアウト)
~~~

* ビルド

~~~
$ make
~~~

### PATH の設定
* Erlang と Elixir の bin に path を通す

## node.js

~~~
$ curl -O https://nodejs.org/dist/v5.5.0/node-v5.5.0-linux-x64.tar.xz
$ xz -dv node-v5.5.0-linux-x64.tar.xz
$ tar -xvf node-v5.5.0-linux-x64.tar
~~~

## inotify-tools

~~~
$ sudo yum install epel-release
$ sudo yum --enablerepo=epel install inotify-tools
~~~

## PhoenixFramework

~~~
$ mix local.hex
$ mix archive.install https://github.com/phoenixframework/archives/raw/master/phoenix_new.ez
~~~


## PostgreSQL の設定

~~~
$ sudo yum install postgresql postgresql-server postgresql-libs postgresql-devel postgresql-contrib
$ sudo mkdir -p /var/lib/pgsql/data
$ sudo chown postgres:postgres /var/lib/pgsql/data
$ sudo su postgres
$ initdb --encoding=UTF8 --pgdata=/var/lib/pgsql/data
$ sudo systemctl start postgresql.service
$ exit
$ psql -U postgres
~~~


## Phoenix プロジェクト作成

~~~
$ mix phoenix.new hello_phoenix
$ cd hello_phoenix/
$ mix phoenix.server
~~~

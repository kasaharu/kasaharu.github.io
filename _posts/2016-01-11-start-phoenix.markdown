---
layout: post
title:  "Elixir と Phoenix を使った Web アプリの始め方"
date:   2016-01-11 12:00:00 +0900
categories: elixir
---

# Elixir のインストール
* Homebrew でインストール

```
$ brew install elixir
```

# Hex のインストール
* Elixir で使用するパッケージ管理ツール Hex をインストール

```
$ mix local.hex
```

# Phoenix のインストール
* 公式ページの [installation](http://www.phoenixframework.org/docs/installation#section-phoenix) にあるようにインストール

```
$ mix archive.install https://github.com/phoenixframework/phoenix/releases/download/v1.1.2/phoenix_new-1.1.2.ez
```

# アプリケーション作成
* myblog という名前でプロジェクトを作成

```
$ mix phoenix.new myblog
```

# サーバの起動とアクセス
* サーバの起動

```
$ mix phoenix.server
```

* サーバへアクセス
    * http://localhost:4000 にアクセス

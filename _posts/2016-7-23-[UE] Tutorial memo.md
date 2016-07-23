---
layout: post
title: リアルタイムJSフレームワーク「Meteor」
permalink: /2014-11-14-01/
---


エンジニアの松本です。

今日は最近Ver1.0になったJSフレームワーク、Meteorを触ってみたので、その共有をしたいと思います。

## Meteorとは

データのリアルタイム同期、クライアントサイド／サーバサイドの同一言語開発(JavaScript、Node.js)、フルスタックフレームワークである事に加えPaaS環境やDB環境まで備えた全部入りの特徴を持ったJSフレームワークです。<br>
2年前ぐらいに初期バージョンが公開され、わりと話題になりました。

で、ようやくバージョンが1.0になったようなので、チュートリアルをやってみました。

作業ログだけ見たい方はこちら→ [Meteor Getting Started & Tutorial - Qiita](http://qiita.com/shu_0115/items/ba73c979cd8cf8152219)

作業後のデプロイ済みの状態はこちら→ [simple-todos](http://simple-todos-sample.meteor.com/)
(複数ブラウザで操作してみると変更がリアルタイムで同期されるのが分かると思います。)

## セットアップ

* [Installing Meteor](https://www.meteor.com/install)

インストールはコマンド一行です。

```
curl https://install.meteor.com/ | sh
------------------------------
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  6117    0  6117    0     0   2958      0 --:--:--  0:00:02 --:--:--  2959
Downloading Meteor distribution
######################################################################## 100.0%

Meteor 1.0 has been installed in your home directory (~/.meteor).
Writing a launcher script to /usr/local/bin/meteor for your convenience.

To get started fast:

  $ meteor create ~/my_cool_app
  $ cd ~/my_cool_app
  $ meteor

Or see the docs at:

  docs.meteor.com
------------------------------
```

## サンプルアプリ作成

* [Creating your first app](https://www.meteor.com/try)

`rails new`と同じように`meteor create`でアプリケーションのひな形を自動生成してくれます。

```
cd ~/WORK_DIR
meteor create simple-todos
------------------------------
simple-todos: created.                        

To run your new app:                          
   cd simple-todos
   meteor
------------------------------
```

* アプリ起動

ローカルでの起動も`rails s`と同じように`meteor`と打つだけです。

```
cd simple-todos
meteor
------------------------------
[[[[[ ~/labo/meteor/simple-todos ]]]]]        

=> Started proxy.                             
=> Started MongoDB.                           
=> Started your app.                          

=> App running at: http://localhost:3000/
------------------------------
```

## データ格納

* [Storing tasks in a collection](https://www.meteor.com/try/3)

データを表示するhtmlとデータを取得するjsを用意します。

* [simple-todos.html](https://gist.github.com/shu0115/710d955200ef40c26bd2)

* [simple-todos.js](https://gist.github.com/shu0115/0d46a1c997a7b7648b7b)

特に設定を書かなくてもMongoに繋がります。

* mongoDBコンソール起動

```
meteor mongo
```

* データ格納

```
db.tasks.insert({ text: "Hello world!", createdAt: new Date() });
```

データが追加された事を勝手に検知して、画面をリロードしなくても追加したデータが画面に表示されます。

## フォームからデータ追加

* [Adding tasks with a form](https://www.meteor.com/try/4)

フォームからデータを追加する場合もhtmlとjsだけで完結します。

* [simple-todos.html](https://gist.github.com/shu0115/efdd82510f7c9aa03baf)

* [simple-todos.js](https://gist.github.com/shu0115/6d943b19878ccb7d63a3)

## データ削除

* [Checking off and deleting tasks](https://www.meteor.com/try/5)

データの削除も、1つのページで削除を実行したらリアルタイムに他の画面にも反映されます。

* [simple-todos.html](https://gist.github.com/shu0115/2b5495328f8dadbad83c)

* [simple-todos.js](https://gist.github.com/shu0115/9e19215ce4460eda331f)

## デプロイ

* [Deploying your app](https://www.meteor.com/try/6)

MeteorはPaaS環境も備えているので、deployコマンドを実行するだけで他の人も閲覧可能な公開環境を作る事が出来ます。

```
meteor deploy simple-todos-sample.meteor.com
------------------------------
To instantly deploy your app on a free testing server, just enter your
email address!

Email: s.matsumoto0115@gmail.com
Deploying to http://simple-todos-sample.meteor.com.
Now serving at http://simple-todos-sample.meteor.com
                                             /
You can set a password on your account or change your email address at:
https://www.meteor.com/setPassword?C3mZnbHWKK
------------------------------
```

### セッションに一時情報保存

* [Storing temporary UI state in Session](https://www.meteor.com/try/8)

セッションを使う場合は`Session.set`、`Session.get`などで。

* [simple-todos.html](https://gist.github.com/shu0115/d031a0806a3e6932e958)

* [simple-todos.js](https://gist.github.com/shu0115/154d707afc9f11552ac2)

## ユーザアカウント機能

* [Adding user accounts](https://www.meteor.com/try/9)

Meteorはパッケージをインストールする事で色々な機能を追加できるらしいです。

ユーザの登録やログイン機能などを追加する場合は↓こんな感じです。

```
meteor add accounts-ui accounts-password
------------------------------
  added accounts-password at version 1.0.4    
  added accounts-base at version 1.1.2        
  added less at version 1.0.11                
  added accounts-ui at version 1.1.3          
  added service-configuration at version 1.0.2
  added accounts-ui-unstyled at version 1.1.4 
  added email at version 1.0.4                
  added sha at version 1.0.1                  
  added localstorage at version 1.0.1         
  added srp at version 1.0.1                  
  added npm-bcrypt at version 0.7.7           

accounts-ui: Simple templates to add login widgets to an app
accounts-password: Password support for accounts
------------------------------
```

```
meteor
------------------------------
[[[[[ ~/labo/meteor/simple-todos ]]]]]        

=> Started proxy.                             
=> Started MongoDB.                           
=> Started your app.                          

=> App running at: http://localhost:3000/
------------------------------
```

* [simple-todos.html](https://gist.github.com/shu0115/60133b7872741a5b047c)

* [simple-todos.js](https://gist.github.com/shu0115/2cf5b9ed0c9834436aac)

## 雑感

何もしなくてもリアルタイム同期されるっていうのはわりと感動的でした。<br>
シングルページのWebアプリケーションやチャットなどのリアルタイム性が重要なアプリケーションの開発にはかなり可能性はあるんじゃないかと思います。

ただ、フルスタックでDBやPaaS環境も内包したようなフレームワークになってるので、わりと使う場面、力を発揮出来るケースが限定されるかもっていう気もします。<br>
クライアントサイドをMeteorで、サーバサイドをRailsで、データのやりとりはAPIで、という構成も出来なくはなさそうなので、機会があればやってみたいなと思います。

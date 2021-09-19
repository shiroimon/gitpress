---
date   : 2021-09-18
title  : 【PostgreSQL】環境設定からDB作成まで
excerpt: 
tags   : ["PostgresSQL", "SQL", "環境設定"]
---

## || PostgreSQLって

## || 環境構築

1. インストール
```
$ brew install postgresql
```

2. バージョン確認
```
$ psql --version
psql (PostgreSQL) 12.3
```
*⚠︎* ここで表示が `not found` なら、パスを通すこと！

Cf. [Mac psqlコマンドのPATHを通す - テンプレ部](https://awesomecatsis.com/mac-psql-path/)

* `locate`コマンド=マッチした条件のファイルを一覧表示
```
$ locate psql | grep /bin
```
*  `.bash_profile` （or `.zchrc`）の有無を確認
```
$ ls -la~
```
* `.bash_profile` の作成
```
$ touch ~/.bash_profile
```
* `.bash_profile` をお好きなエディタで開封(ここではvimを使用)
```
$ vi ~/.bash_profile    （or .zchrc）
```
`export` から記入。（パスを通す作業）
```
[i] 入力モード
export PATH=$PATH:/Library/PostgreSQL/{バージョン}/bin
[esc] > [:wq] 保存  > [Enter]
```
* `.bash_profile` をリロード
```
$ source ~/ .bash_profile
（$ exec $SHELL -l）
```
最後にパスが通ったか確認で、もう一度バージョン確認してみる。通過していればバージョンが返ってくる。

3. 起動
```
$ psql -U postgres -h localhost -W
$ 🗝
postgres=#
```
以下の記述でもログインできる。
```
$ psql -h ホスト名 -p ポート番号(5432) -U ロール名 -d データベース名
```
Cf. [PostgreSQLへの接続と切断 - DBOnline](https://www.dbonline.jp/postgresql/connect/index2.html)



※ 文頭に `postgres=#` と出ていれば起動成功。

## || 起動後

* データベース作成
```
postgres=# creat database {任意のDB名};
```

* データベース一覧（List）
```
postgres=# \l
```

* データベース削除
```
postgres=# drop database {DB名};
```

* PostgreSQL終了
```
postgres=# \q
```

Cf. [PostgreSQLの使い方 - DBOnline](https://www.dbonline.jp/postgresql/#section_ini)

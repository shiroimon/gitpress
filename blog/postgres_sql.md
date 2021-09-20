---
date   : 2021-09-18
title  : 【PostgreSQL】環境設定からDB作成まで
excerpt:🐘
tags   : ["PostgresSQL", "SQL", "環境設定"]
---

## || PostgreSQLって

## || 環境構築

1. インストール
```linux
$ brew install postgresql
```

2. バージョン確認
```linux
$ psql --version
psql (PostgreSQL) 12.3
```
*⚠︎* ここで表示が `not found` なら、パスを通すこと！

Cf. [Mac psqlコマンドのPATHを通す - テンプレ部](https://awesomecatsis.com/mac-psql-path/)

* `locate`コマンド=マッチした条件のファイルを一覧表示
```linux
$ locate psql | grep /bin
```
*  `.bash_profile` （or `.zchrc`）の有無を確認
```linux
$ ls -la~
```
* `.bash_profile` の作成
```linux
$ touch ~/.bash_profile
```
* `.bash_profile` をお好きなエディタで開封(ここではvimを使用)
```linux
$ vi ~/.bash_profile    （or .zchrc）
```
`export` から記入。（パスを通す作業）
```linux
[i] 入力モード
export PATH=$PATH:/Library/PostgreSQL/{バージョン}/bin
[esc] > [:wq] 保存  > [Enter]
```
* `.bash_profile` をリロード
```linux
$ source ~/ .bash_profile
（$ exec $SHELL -l）
```
最後にパスが通ったか確認で、もう一度バージョン確認してみる。通過していればバージョンが返ってくる。

3. 起動
```linux
$ psql -U postgres -h localhost -W
$ 🗝
postgres=#
```
以下の記述でもログインできる。
```linux
$ psql -h ホスト名 -p ポート番号(5432) -U ロール名 -d データベース名
```
Cf. [PostgreSQLへの接続と切断 - DBOnline](https://www.dbonline.jp/postgresql/connect/index2.html)



※ 文頭に `postgres=#` と出ていれば起動成功。

## || 起動後

* データベース作成
```linux
postgres=# creat database {任意のDB名};
```

* データベース一覧（List）
```linux
postgres=# \l
```

* データベース削除
```linux
postgres=# drop database {DB名};
```

* 作成したテーブルにアクセス
```linux
$ psql -d {DB名} -U postgres
{DB名}=#
```

* 作成済みのテーブル一覧
```linux
{DB名}=# \dt
```

* PostgreSQL終了
```linux
postgres=# \q
```

Cf. [PostgreSQLの使い方 - DBOnline](https://www.dbonline.jp/postgresql/#section_ini)

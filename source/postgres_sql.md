---
date    : 2021-09-18
title   : 【🐘 PostgreSQL】環境設定からDB作成まで
excerpt :
tags    : ["PostgresSQL", "SQL", "環境設定", "iTearm2"]
---

## || PostgreSQLって
![img](http://road-to-tennis.sakura.ne.jp/circles/wp-content/uploads/2018/01/postgresql-logo.png)
> PostgreSQLは、拡張性とSQL準拠を強調するフリーでオープンソースの関係データベース管理システムである。Postgresとしても知られている。もともとは、カリフォルニア大学バークレー校で開発されたIngresデータベースの後継としてその起源を根拠としたPOSTGRESという名前であった。1996年に、プロジェクトはSQLのサポートを反映してPostgreSQLに改名された。2007年の検討の結果、開発チームはPostgreSQLという名前とPostgresという別名を維持することを決定した。
>PostgreSQLは、原子性、整合性、独立性、耐久性 (ACID)プロパティを持つトランザクション、自動更新可能なビュー、マテリアライズドビュー、トリガ、外部キー、ストアドプロシージャを特徴としている。単一マシンからデータウェアハウスや多数の同時使用ユーザを持つWebサービスまで、さまざまなワークロードを扱えるように設計されている。macOS Serverのデフォルトデータベースであり、Linux、FreeBSD、OpenBSD、Windowsでも利用可能である。
>
> ー [PostgreSQL - Wikipedia](https://ja.wikipedia.org/wiki/PostgreSQL)


## || 環境構築
1. インストール
```shell
$ brew install postgresql
```

2. バージョン確認
```shell
$ psql --version
psql (PostgreSQL) 12.3
```

*⚠︎* ここで表示が `not found` なら、パスを通すこと！
* `locate`コマンド=マッチした条件のファイルを一覧表示
```shell
$ locate psql | grep /bin
```
*  `.bash_profile` （or `.zchrc`）の有無を確認
```shell
$ ls - la
```
* `.bash_profile` の作成
```shell
$ touch ~/.bash_profile
```
* `.bash_profile` をお好きなエディタで開封(ここではvimを使用)
```shell
$ vi ~/.bash_profile    （or .zchrc）
```
`export` から記入。（パスを通す作業）
```shell
[i] 入力モード
export PATH=$PATH:/Library/PostgreSQL/{バージョン}/bin
[esc] > [:wq] 保存  > [Enter]
```
* `.bash_profile` をリロード
```shell
$ source ~/ .bash_profile
（$ exec $SHELL -l）
```
        最後にパスが通ったか確認で、もう一度バージョン確認してみる。
        通過していればバージョンが返ってくる。
Cf. [Mac psqlコマンドのPATHを通す - テンプレ部](https://awesomecatsis.com/mac-psql-path/)

3. ログイン、ログアウト
```shell
$ psql -h ホスト名 -p ポート番号(5432) -U ロール名 -d データベース名
```
|||||
|-|-|-|-|
|-h|ホスト名|localhost|localhostであれば省略可|
|-p|ポート番号|5432|基本であれば省略可|
|-U|ロール名|postgres|インストール直後のsuper user|
|-d|データベース名|postgres|インストール直後デフォルト|
|-W||||

* PostgreSQLログイン
```SHELL
psql -h localhost -p 5432 -U postgres -d postgres
Password:🗝
postres=#
```
省略パターン
```shell
$ psql -U postgres
```
※ 文頭に `postgres=#` と出ていれば起動成功。

* PostgreSQLログアウト
```shell
postgres=# \q
```


Cf. [PostgreSQLへの接続と切断 - DBOnline](https://www.dbonline.jp/postgresql/connect/index2.html)


## || データベースアクセス
```SHELL
psql -h localhost -p 5432 -U postgres -d postgres
Password:🗝
postres=#
```

|||
|-|-|
|データベース作成|`creat database {DB名}` |
|データベース削除|`drop database {DB名}` |
|データベース一覧確認|`\l` |
|ロール確認|`\du` |
|別のデータベースにアクセス|`\c {別DB名}`|
|現在接続しているDB名取得|`select current_database();`|
|DB名変更|`ALTER DATABASE {DB名} RENAME TO {New_DB名};`|
|DB所有者変更|`ALTER DATABASE {DB名} OWNER TO { new_owner | CURRENT_USER | SESSION_USER };`|

* テーブル作成（e.g. 都市と地理的な位置情報を格納）
```shell
CREATE TABLE {TB名: cities} (
    name      varchar(80),
    location  point
);
```
      ※ point型は、PostgreSQL独自のデータ型

* テーブル削除
```shell
DROP TABLE {TB名};
```


## || テーブルアクセス
```shell
$ psql -U postgres -d {TB名}
```

* 作成済みのテーブル一覧
```shell
{TB名}=# \dt
```





Cf.
* [PostgreSQLの使い方 - DBOnline](https://www.dbonline.jp/postgresql/#section_ini)
* [実際のSQL環境のシミュレーション - ICHI.PRO](https://ichi.pro/jissai-no-sql-kankyo-no-shimyure-shon-109368380962734)
* [Hands-on SQL in PostgreSQL - DatabaseJournal](https://www.databasejournal.com/features/postgresql/hands-on-sql-in-postgresql.html)
* [SQLの練習に最適！ブラウザ上で実行できる初心者向け学習サービス5選 - paiza](https://paiza.hatenablog.com/entry/2019/12/22/SQL%E3%81%AE%E7%B7%B4%E7%BF%92%E3%81%AB%E6%9C%80%E9%81%A9%EF%BC%81%E3%83%96%E3%83%A9%E3%82%A6%E3%82%B6%E4%B8%8A%E3%81%A7%E5%AE%9F%E8%A1%8C%E3%81%A7%E3%81%8D%E3%82%8B%E5%88%9D%E5%BF%83%E8%80%85%E5%90%91)
* [[PostgreSQL] サンプルのデータベースを用意する - DevelopersIO](https://dev.classmethod.jp/articles/postgresql-create-sample-database/)

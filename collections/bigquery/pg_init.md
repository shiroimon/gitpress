---
date    : 2021-09-18
title   : 🐘 PostgreSQL 
excerpt : ---
tags    : ["🐘", "PostgreSQL", "OSS-DB", "SQL", "環境構築", "iTearm2"]
---

![img](https://i.gyazo.com/8bb756a9d2e944bba0e29344976093bd.jpg)

## || PostgreSQL とは？
> PostgreSQLは、拡張性とSQL準拠を強調するフリーでオープンソースの関係データベース管理システムである。
> Postgresとしても知られている。もともとは、カリフォルニア大学バークレー校で開発されたIngresデータベースの後継としてその起源を根拠としたPOSTGRESという名前であった。
> 1996年に、プロジェクトはSQLのサポートを反映してPostgreSQLに改名された。2007年の検討の結果、開発チームはPostgreSQLという名前とPostgresという別名を維持することを決定した。
> PostgreSQLは、原子性、整合性、独立性、耐久性 (ACID)プロパティを持つトランザクション、自動更新可能なビュー、マテリアライズドビュー、トリガ、外部キー、ストアドプロシージャを特徴としている。単一マシンからデータウェアハウスや多数の同時使用ユーザを持つWebサービスまで、さまざまなワークロードを扱えるように設計されている。
> macOS Serverのデフォルトデータベースであり、Linux、FreeBSD、OpenBSD、Windowsでも利用可能である。
>
> "[PostgreSQL](https://ja.wikipedia.org/wiki/PostgreSQL) - Wikipedia"



## || 環境構築
### | インストール
* インストール
```shell
$ brew install postgresql
```
* バージョン確認
```shell
$ psql --version
psql (PostgreSQL) 12.3 # `⚠︎` ここで表示が `not found` なら、パスを通すこと！
```
* `locate`コマンド=マッチした条件のファイルを一覧表示
```shell
$ locate psql | grep /bin
```
*  `.bash_profile` （or `.zshrc`）の有無を確認
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
# [i] 入力モード
export PATH=$PATH:/Library/PostgreSQL/{バージョン}/bin
# [esc] > [:wq] 保存  > [Enter]
```
* `.bash_profile` をリロード
```shell
$ source ~/ .bash_profile # $ exec $SHELL -l
```
最後にパスが通ったか確認で、もう一度バージョン確認してみる。
通過していればバージョンが返ってくる。

cf. [Mac psqlコマンドのPATHを通す - テンプレ部](https://awesomecatsis.com/mac-psql-path/)


### |  ログイン、ログアウト
* ログイン
```shell
$ psql -h ホスト名 -p ポート番号(5432) -U ロール名 -d データベース名
```
|||||
|:-:|:-|:-|:-|
|-h|ホスト名|localhost|localhostであれば省略可|
|-p|ポート番号|5432|基本であれば省略可|
|-U|ロール名|postgres|インストール直後のsuper user|
|-d|データベース名|postgres|インストール直後デフォルト|
|-W|||起動の完了を待たずにコマンド発行元に制御を戻すオプション|

* PostgreSQLログイン
```shell
$ psql -h localhost -p 5432 -U postgres -d postgres
Password for user postgres:🗝
postgres＝♯
```
省略パターン
```shell
$ psql -U postgres
```

* ログアウト
```shell
postgres＝♯ \q
```

cf. [PostgreSQLへの接続と切断 - DBOnline](https://www.dbonline.jp/postgresql/connect/index2.html)


* ログイン前にエラーが出る
```txt
psql: error: connection to server on socket "/tmp/.s.PGSQL.5432" failed: No such file or directory
	Is the server running locally and accepting connections on that socket?
```
```shell
# PostgreSQLがインストールされているか確認
$ psql --version
psql (PostgreSQL) 14.10 (Homebrew)
# (1度目) DB一覧を確認
$ psql -l
psql: error: connection to server on socket "/tmp/.s.PGSQL.5432" failed: No such file or directory
	Is the server running locally and accepting connections on that socket?
# 
$ brew services start postgresql
$ brew services list
# (2度目)DB一覧を確認
$ psql -l
                                        List of databases
   Name    |      Owner      | Encoding | Collate | Ctype |            Access privileges
-----------+-----------------+----------+---------+-------+-----------------------------------------
 postgres  | XXX             | UTF8     | C       | C     |
```

`cf.`
* [PostgreSQL接続時に「psql: error: connection to server on socket "/tmp/.s.PGSQL.5432" failed:」エラーが出た時の解消](https://qiita.com/zazaza/items/02d4ac1499ed671d3b93) - Qiita
* [PostgreSQLでFATAL: role “postgres” does not existが発生した場合の原因と対処法](https://lifehack.world/postgresql-fatal-role-postgres-does-not-exist/) - Hoehoe Blog
* [MacでPostgreSQLをインストールする](https://zenn.dev/eguchi244_dev/articles/sql-postresql-install-20230620) - Zenn 





**cf.**
* [PostgreSQLの使い方 - DBOnline](https://www.dbonline.jp/postgresql/#section_ini)
* [実際のSQL環境のシミュレーション - ICHI.PRO](https://ichi.pro/jissai-no-sql-kankyo-no-shimyure-shon-109368380962734)
* [Hands-on SQL in PostgreSQL - DatabaseJournal](https://www.databasejournal.com/features/postgresql/hands-on-sql-in-postgresql.html)
* [SQLの練習に最適！ブラウザ上で実行できる初心者向け学習サービス5選 - paiza](https://paiza.hatenablog.com/entry/2019/12/22/SQL%E3%81%AE%E7%B7%B4%E7%BF%92%E3%81%AB%E6%9C%80%E9%81%A9%EF%BC%81%E3%83%96%E3%83%A9%E3%82%A6%E3%82%B6%E4%B8%8A%E3%81%A7%E5%AE%9F%E8%A1%8C%E3%81%A7%E3%81%8D%E3%82%8B%E5%88%9D%E5%BF%83%E8%80%85%E5%90%91)
* [[PostgreSQL] サンプルのデータベースを用意する - DevelopersIO](https://dev.classmethod.jp/articles/postgresql-create-sample-database/)
* [「WARNING: psql version 9.2, server version 11.0.　Some psql features might not work.」エラー対処(AWSLinux(EC2)からpsql11のインストール) - Qiita](https://qiita.com/aomo_study/items/cab07f00a349bf68dee3)





---
# (番外編) PostgreSQLにおける、データベース＞スキーマ＞テーブル

> ![img](https://www.dbonline.jp/postgresql/schema/img/p1-5.png)
> ー [PostgreSQLにおけるデータベース、スキーマ、テーブルの関係 - DBOnline](https://www.dbonline.jp/postgresql/schema/index1.html)

## || データベース
```SHELL
$ psql -h localhost -p 5432 -U postgres -d postgres
Password for user postgres:🗝
psql (12.3)

postgres＝♯ \l    #データベース一覧
postres＝♯ create database mydb;    #データベース作成
CREATE DATABASE
postres＝♯
```

|||
|-|-|
|データベース作成|`create database {DB名};` |
|データベース削除|`drop database {DB名};` |
|データベース一覧確認|`\l` |
|ロール確認|`\du` |
|別のデータベースにアクセス|`\c {別DB名}`|
|現在接続しているDB名取得|`select current_database();`|
|DB名変更|`ALTER DATABASE {DB名} RENAME TO {New_DB名};`|
|DB所有者変更|`ALTER DATABASE {DB名} OWNER TO { new_owner, CURRENT_USER, SESSION_USER };`|


## || スキーマ
```SHELL
$ psql -U postgers
Password: 🗝
psql (12.3)

postgres＝♯ \c mydb    #データベース移動
You are now connected to database "mydb" as user "postgres".
mydb＝♯ \dn    #スキーム一覧
List of schemas
Name    |  Owner
--------+----------
public  | postgres    #デフォルトで入っている
(1 row)

mydb＝♯ create schema myschema;    #スキーム作成
CREATE SCHEMA
mydb＝♯ \dn
  List of schemas
  Name      |  Owner
------------+----------
 public     | postgres
 myschema   | postgres
(2 rows)

```

|||
|-|-|
|スキーマ作成|`CREATE SCHEMA {スキーマ名};`|
|スキーマ作成（所有者指定:ロール）|`CREATE SCHEMA {スキーマ名} AUTHORIZATION {所有者名};`|
|<p>スキーマ作成（同時にオブジェクト生成）</p><p>e.g. テーブル作成等</p> |<p>`CREATE SCHEMA {スキーマ名} {schema_element}`</p> ```CREATE TABLE 、 CREATE VIEW 、 CREATE INDEX 、 CREATE SEQUENCE 、 CREATE TRIGGER 、 GRANT```|
|スキーマ一覧|`\dn`|
|スキーマ一覧(アクセス権限も含めて表示)|`\dn+`|
|スキーマ指定（テーブル呼出）|`SELECT * FROM {スキーマ名}.{テーブル名};`|
|スキーマ変更（名前）|`ALTER SCHEMA {スキーマ名} RENAME TO {New_スキーマ名};`|
|スキーマ変更（所有者）|`ALTER SCHEMA {所有者} OWNER TO {New_所有者};`|
|スキーマ削除|`DROP SCHEMA {スキーマ名};`|


## || テーブル

```shell
$ psql -U postgres -d mydb
Password: 🗝
psql (12.3)
# 現在のスキーム確認（デフォルトのpublicにテーブル作成）
mydb＝♯ select current_schema;
current_schema
--------------
public
(1 row)

# テーブル作成（e.g. 都市と地理的な位置情報を格納）
mydb＝♯ CREATE TABLE mytbl1 (id integer, name varchar(80), location point);
CREATE TABLE
mydb＝♯ \dt

# 作成済みスキーマ確認
mydb＝♯ \dn

# 指定スキーマ内にテーブル作成
mydb＝♯ CREATE TABLE myschema.mytbl2 (id integer, name varchar(10));
CREATE TABLE

# スキーマ内のテーブル一覧
mydb＝♯ \dt myschema.*
```

      ※ point型は、PostgreSQL独自のデータ型

|||
|-|-|
|テーブル作成|`CREATE TABLE {}();`|
|テーブル削除|`DROP TABLE {tb名};`|
|テーブル一覧|`{TB名}=# \dt`|
| |``|



## || pg_{ } システムカタログ

    システムカタログとは PostgreSQL の管理システムが使用するテーブルで、
    データベースやテーブルなどの情報を管理するために使用。
    (pg_namespace, pg_authid, pg_roles, pg_tables, pg_setting...etc.)


**e.g.**
システムカタログから、現在接続しているデータベースに含まれるスキーマの一覧を取得。
( `pg_` で始まるスキーマ、`information_schema`は、PostgreSQL のシステムが使用しているもの)

```SHELL
postres＝♯ SELECT * FROM pg_namespace;

  oid  |      nspname       | nspowner |               nspacl
-------+--------------------+----------+-------------------------------------
    99 | pg_toast           |       10 |
 12314 | pg_temp_1          |       10 |
 12315 | pg_toast_temp_1    |       10 |
    11 | pg_catalog         |       10 | {postgres=UC/postgres,=U/postgres}
  2200 | public             |       10 | {postgres=UC/postgres,=UC/postgres}
 13337 | information_schema |       10 | {postgres=UC/postgres,=U/postgres}
(6 rows)
```

|カラム|型|説明|
|-|-|-|
|oid|oid|識別子(明示的に指定しないと取得できません)|
|nspname|name|名前空間の名前|
|nspowner|oid|名前空間の所有者|
|nspacl|aclitem[]|アクセス権限の一覧|


**e.g.**
システムカタログの一つである `pg_roles` からロールの一覧を取得。
```shell
postres＝♯ SELECT * FROM pg_roles;

rolname                   | rolsuper | rolinherit | rolcreaterole | rolcreatedb | rolcanlogin | rolreplication | rolconnlimit | rolpassword | rolvaliduntil | rolbypassrls | rolconfig | oid
---------------------------+----------+------------+---------------+-------------+-------------+----------------+--------------+-------------+---------------+--------------+-----------+------
pg_signal_backend         | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 4200
pg_read_server_files      | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 4569
postgres                  | t        | t          | t             | t           | t           | t              |           -1 | ********    |               | t            |           |   10
pg_write_server_files     | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 4570
pg_execute_server_program | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 4571
pg_read_all_stats         | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 3375
pg_monitor                | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 3373
pg_read_all_settings      | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 3374
pg_stat_scan_tables       | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 3377
(9 rows)
```

|カラム|型|説明|
|-|-|-|
|rolname|	name|	ロール名|
|rolsuper|	bool|	スーパーユーザの権限の有無|
|rolinherit|	bool|	メンバであるロールの権限を継承するかどうか|
|rolcreaterole|	bool|	ロールの作成権限の有無|
|rolcreatedb|	bool|	データベースの作成権限の有無|
|rolcanlogin|	bool|	ログインを行えるロールかどうか|
|rolreplication|	bool|	レプリケーション用のロールかどうか|
|rolconnlimit|	int4|	同時接続の最大数(-1は無制限)|
|rolpassword|	text|	パスワード(ただし ******** とのみ表示)|
|rolvaliduntil|	timestampt|	パスワードの有効期限(有効期限がない場合はNULL)|
|rolbypassrls|	bool|	すべての行単位セキュリティポリシーを無視するかどうか|
|rolconfig|	text[]|	実行時設定変数に関するロール固有のデフォルト|
|oid|	oid|	ロールのID|

→ デフォのロールを除いて、作成したロールの一覧を取得する方法。
```SQL
SELECT rolname, rolsuper, rolcanlogin
FROM  pg_roles
WHERE rolname NOT LIKE "PG_%";
```

**e.g.**
```SHELL
postgres＝♯ SELECT * FROM pg_tables;

```
|名前|	データ型|	参照先|	説明|
|-|-|-|-|
|schemaname|	name|	pg_namespace.nspname|	スキーマ名|
|tablename|	name|	pg_class.relname|	テーブル名|
|tableowner|	name|	pg_authid.rolname|	所有者|
|tablespace|	name|	pg_tablespace.spcname	テーブルを含むテーブル空間の名前（データベースのデフォルトの場合はNULL）|
|hasindexes|	boolean|	pg_class.relhasindex|	テーブルがインデックスを持っている（もしくは最近まで持っていた）なら真|
|hasrules|	boolean|	pg_class.relhasrules|	テーブルにルールがある（もしくは以前あった）時は真|
|hastriggers|	boolean|	pg_class.relhastriggers|	テーブルにトリガがある（もしくは以前あった）時は真|
|rowsecurity|	boolean|	pg_class.relrowsecurity|	テーブルの行セキュリティが有効なら真|

```SQL
SELECT schemaname, tablename,tableowner
FROM pg_tables
WHERE
    schemaname NOT LIKE "pg_%"
    AND
    schemaname != "information_schema";
```



## || ロール
```shell
postres＝♯ CREATE ROLE new_roal WITH LOGIN PASSWORD 'password';
CREATE ROAL
postres＝♯ \du    #ロール一覧
List of roles
Role name  |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
postgres   | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
new_roal   |                                                            | {}
postres＝♯ \q
$ psql -U new_roal -d mydb    #新しいロール（user）再接続
Password for user new_roal:🗝     #passwordと入力
psql(12.3)
msdb＝＞
```

|||
|:-|:-|
|ロール一覧|`\du`|
|ロール新規作成|`CREATE ROLE {ロール名} WITH LOGIN PASSWORD {password};`|
|ロール新規作成(オプション指定)|`CREATE ROLE {ロール名} WITH {Option} LOGIN PASSWORD {password};`|
|ロール変更（名前）|`ALTER ROLE {ロール名} RENAME TO {New_ロール名}`|
|ロール変更（属性）|`ALTER ROLE {ロール名} WITH {Option}`|
|ロール設定（パス有効期限）|`CREATE ROLE {ロール名} WITH LOGIN PASSWORD {password}` `VALID UNITIL "timestamp(or"infinity
|テーブル、カラム権限一覧|`\dp`|



* 権限付与（オプション指定）
```shell
postres＝♯ CREATE ROAL momo WITH createdb createrole LOGIN PASSWORD 'peach';
```

※ オプションの数は割と多いので、使用したら適宜追記。

|||
|-|-|
|Superuser|スーパーユーザーはほとんどの権限を持つことになるので作成する場合は注意が必要。尚 PostgreSQL インストール時に自動で作成される `postgres ロール`はスーパーユーザー。 `NOSUPERUSER` を指定した場合はスーパーユーザーではないロールを作成可能。|
|Create role|指定した場合、作成するロールはロール作成権限を持つ。 `NOCREATEROLE` を指定した場合はロールをを作成することができない。**※ CREATEROLE 権限を付与する場合は注意が必要**。 CREATEROLE 権限があるロールは、自身が持っていない権限を持つようなロールを新たに作成することができる。（勝手に`Superuser`並の権限ロールを作られたら...）。このオプションを指定しなかった場合は、デフォルトで NOCREATEROLE が指定される。|
|Create DB|CREATEDB を指定した場合、作成するロールはデータベースを作成する権限を持つ。 `NOCREATEDB` を指定した場合はデータベースを作成することができない。(デフォルトで `NOCREATEDB` が指定される。)|
|Replication||
|Bypass RLS||
|Inherit||
|Login||
|Connection||
|VALID UNTIL 'timestamp'||
|IN ROLE {ロール名}||
|IN GROUP {ロール名}||
|ROAL {ロール名}||
|ADMIN {ロール名}||
|USER {ロール名}||
|SYSID uid||

cf. [ロールを作成する(CREATE ROLE)](https://www.dbonline.jp/postgresql/role/index2.html)



## || REFERNCE
- [PostgreSQLのロール・権限のここがわかりづらい](https://speakerdeck.com/tameguro/postgresqlnororuquan-xian-nokokogawakaridurai) - speakerdeck
- [PostgreSQL で時系列データの扱いを考える](https://zenn.dev/hasegawasatoshi/articles/7659dc2b9a0081) - Zenn
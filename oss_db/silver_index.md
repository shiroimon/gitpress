---
date   : 2021-09-29
title  : 【Silver Ver.2.0】
excerpt:
tags   : ["OSS-DB", "Silver", "PostgreSQL"]
---
## || INDEX
    0. 概要
        - OSS-DBとは
        - 戦略
    1. 【一般知識】
        1-1. OSS -DBの一般的特徴
        2-2. リレーショナルデータベースに関する一般知識
    2. 【運用管理】
        2-1. インストール方法
        2-2. 標準付属ツールの使い方
        2-3. 設定ファイル
        2-4. バックアップ方法
        2-5. 基本的な運用管理作業
    3. 【開発 / SQL】
        3-1. SQLコマンド
        3-2. 組み込み関数
        3-3. トランザクションの概念

## || 0. 概要
### | OSS-DBとは
 ![img](https://lpi.or.jp/assets/img/common/ossdb_logo.png)
 * [OSS-DB Silver Ver.2.0 - OSS-DB](https://oss-db.jp/outline/silver)

        下記のスキルと知識を持つエンジニアであることを証明する。
        RDBMSとSQLに関する知識を有する。
         ・オープンソースデータベースに関する基礎的な知識を有する。
         ・オープンソースを利用して小規模なデータベースの運用管理ができる。
         ・オープンソースを利用して小規模なデータベースの開発を行うことができる。
         ・PostgreSQLを使ったデータベースシステムの運用管理ができる。
         ・PostgreSQLを利用した開発でデータベース部分を担当することができる。

 * [LPI-Japna - official](https://lpi.or.jp/)

        LPI-Japanは、日本での Linuxの技術力認定試験の普及とITプロフェッショナルの育成のため2000年7月に設立され、
        現在は対象を広げOSSのデータベースソフトウェア、クラウドソフトウェアであるCloudStackやOpenStack、
        更にはHTML5のプロフェッショナルのための認定試験を実施するNPO法人です。

#### OSS-DB silver - （公式）無料問題集+α
 + [Silverの例題解説「一般知識」 - OSS DB](https://oss-db.jp/sample/silver_knowledge_01)
 + [Silverの例題解説「運用管理」 - OSS DB](https://oss-db.jp/sample/silver_management_01)
 + [Silverの例題解説「開発/SQL」 - OSS DB](https://oss-db.jp/sample/silver_development_01)

#### OSS-DB silver - リファレンス(Blog)
 + [OSS-DB Silver対策問題集](http://oss-db-taisaku.net/)

#### OSS-DB silver - リファレンス(Youtube)
 + [(2021/06/29)OSS-DB Exam Silver 技術解説セミナー](https://www.youtube.com/watch?v=_sPOt2_TMko)

 + [【Part1】OSS DB Exam Silver 技術解説セミナー ～ 試験の概要、PostgreSQLとは ～ - SRA OSS, inc.](https://www.youtube.com/watch?v=3iR6FiCSTKM)
 + [【Part2】OSS DB Exam Silver 技術解説セミナー ～ PostgreSQL 環境作り ～ - SRA OSS, inc.](https://www.youtube.com/watch?v=tG-8WBNxABI)
 + [【Part3】OSS DB Exam Silver 技術解説セミナー ～ PostgreSQL 基本操作 （起動/停止、ユーザ作成、データベース作成、psql）～ - SRA OSS, inc.](https://www.youtube.com/watch?v=wSkZqw_IVps)
 + [【Part4】OSS DB Exam Silver 技術解説セミナー ～ PostgreSQL 設定ファイル（postgresql.conf）、Q&A ～ - SRA OSS, inc.](https://www.youtube.com/watch?v=QR0d0JTRwAk)
 + [【Part5】OSS DB Exam Silver 技術解説セミナー ～ PostgreSQL 設定ファイル（pg_hba.conf）、VACUUM/ANALYZE ～ - SRA OSS, inc.](https://www.youtube.com/watch?v=FKRXpuYJtPc)
 + [【Part6】OSS DB Exam Silver 技術解説セミナー ～ PostgreSQL バックアップ・リストア PITR ①～ - SRA OSS, inc.](https://www.youtube.com/watch?v=Czx1Kr7OD-Q)
 + [【Part7】OSS DB Exam Silver 技術解説セミナー ～ PostgreSQL バックアップ・リストア PITR ② ～ - SRA OSS, inc.](https://www.youtube.com/watch?v=gP-ripEm4yo)
 + [【Part8】OSS DB Exam Silver 技術解説セミナー ～ Q&A～ - SRA OSS, inc.](https://www.youtube.com/watch?v=WRAjfbHwJ04)

#### PostgreSQL - リファレンス(Blog)
 + [PostgreSQLの使い方 - DBOnline](https://www.dbonline.jp/postgresql/)

#### PostgreSQL - リファレンス(Youtube)
 + [【Part1】PostgreSQL再入門 ～ PostgreSQLの全体像 ～ - SRA OSS, inc.](https://www.youtube.com/watch?v=zIH1dTNPL0A)


#### OSS-DB silver - 合格体験記
 + [【勉強期間：2週間】OSS-DB Silverに合格するまで - 凡人データエンジニア](https://marketingengineercareer.com/oss-db-silver)
 +
 +

### | 戦略
 ![img](https://i.gyazo.com/81486c0db3a863f85f9883241f1bf3cd.png)
 より確度の高いエンジニアと名乗るには、全てを網羅している必要があると思う。
 しかし、試験通過を念頭とするので、試験の性質である出題割合の降順にしてみた。伴って、パワーバランスを明確にし取り組みたい。


## || 1.【一般知識】 (16.0%)
### | 1-1. OSS -DBの一般的特徴 (8.0%)

* [PostgreSQL 12.4文書 - PostgreSQLグローバル開発グループ](https://www.postgresql.jp/document/12/html/index.html)

### | 1-2. リレーショナルデータベースに関する一般知識 (8.0%)

## || 2.【運用管理】 (52.0%)
### | 2-1. インストール方法 (4.0%)


`initdb {オプション名} {ディレクトリ名}`

|オプション名|説明|
|-|-|
|<p>`-D {ディレクトリ名}`</p> <p>`--pgdata {ディレクトリ名}`</p>|データベースクラスタを作成するディレクトリを指定|
|<p>`-E {エンコーディング}`</p> <p>`--encoding {エンコーディング}`</p>|エンコーディングを指定する|
|<p>`--local={ロケール}`</p>|ロケールを指定(OSコマンドを使用するか)|
|<p>`--local=c`</p> <p>`--no-local`</p>|ロケールを無効|
|<p>`-U {ユーザー名}` `--username={ユーザー名}`</p>|作成するデータベースのスーパーユーザー名指定|
|<p>`-k`</p> <p>`--data-cheaksums`</p>|データベースのチェックサム有効（データ破損を検出するための仕組み）|
|<p>`-x {ディレクトリ名}`</p> <p>`--waldir={ディレクトリ名}`</p>|WAL(Write Ahead Logging: 記録変更ログ) 格納するディレクトリを指定|




### | 2-2. 標準付属ツールの使い方 (10.0%)

### | 2-3. 設定ファイル (10.0%)
 + **postgresql.conf**
       postgresql.conf ファイルは PostgreSQL に関する基本的な設定を記述するファイル。
  ■ファイルの場所：

  ■ログの出力内容設定：
  |||
  |-|-|
  |stderr|…サーバログを平文で標準エラー出力に出力|
  |csvlog|…サーバログをCSV形式で標準エラー出力に出力|
  |syslog|…サーバログをsyslogに出力|

  * `logging_collector` = syslog … 標準エラーをファイルに書き出すかどうかを設定するパラメータ。
  * `log_directory` = syslog … ログファイルを格納するディレクトリを設定するパラメータ。
  * `log_destination` = syslog … PostgreSQLサーバのログ出力先を設定するパラメータ。
  * `log_filename` = syslog … サーバログを書き込むファイルのファイル名を設定するパラメータ。
  * `log_connections` = syslog … クライアント認証の成功終了やサーバへの接続試行などの、クライアント情報をサーバログに出力するかどうかを設定。
  * `log_min_messages` = syslog … サーバログに書き込むログのレベルを設定。
  * `log_line_prefix` = syslog … サーバログメッセージの行頭に、ユーザ名やデータベース名、日付などの書式文字列を設定。

 + **pg_hba.conf**
       pg_hba.conf ファイルは PostgreSQL に接続するクライアントの認証に関する設定を記述するファイル。
  ■ファイルの場所：
  ![img](https://i.gyazo.com/b8bfcd4fb8756e2771ad72561644e519.png)
  ```SHELL
  $ psql -U postgres -t -P format=unaligned -c 'show hba_file';
  ```
  Cf. [PostgreSQLのpg_hba.confファイルの場所を探す - Qiita](https://qiita.com/quzq/items/2fbda9fc5f1a2c3e106c)

  ■接続するクライアントの認証設定：

  192.168.1.0/24 のネットワークからデータベース mydb への接続をユーザー yamada に対して認証方式 md5 で許可する場合
  ```text
  # TYPE  DATABASE        USER          ADDRESS               METHOD

  # IPv4 local connections:
  # host    all             all           127.0.0.1/32          md5
  # IPv6 local connections:
  # host    all             all           ::1/128               md5
  # Allow replication connections from localhost, by a user with the
  # replication privilege.
  # host    replication     all           127.0.0.1/32          md5
  # host    replication     all           ::1/128               md5

  host    mydb             postgres       127.0.0.1/32          md5
  host    mydb             yamada         192.168.1.0/24        md5
  ```
  |認証方式|説明|
  |-|-|
  |trust|常に許可|
  |reject|常に拒否|
  |md5|MD5で暗号化されたパスワード認証|
  |password|暗号化されていない平文でのパスワード認証|

 + **pg_ident.conf**
       pg_ident.conf ファイルは クライアントの認証方式として Ident 認証 を使用する場合に、
       ident のユーザー名を PostgreSQL のロール名にマッピングするために使用するファイル。
  ■ファイルの場所：
  ![img](https://i.gyazo.com/e51e949528b227133cf1f3b1862da821.png)
  ```SHELL
  $ psql -U postgres -t -P format=unaligned -c 'show ident_file';
  ```
  ■マッピング追加設定：
  1. `pg_ident.conf` ident で nancy として認証されたユーザーが tom として PostgreSQL に接続が許可。
   ```text
   # Put your actual configuration here
   # ----------------------------------
   # MAPNAME       SYSTEM-USERNAME         PG-USERNAME
   mymap           nancy                   tom
   ```
  2. `pg_hba.conf` マッピングを追加する時に指定した MAPNAME を指定。
  ```text
  host  all  all  127.0.0.1/32   ident map=mymap
  ```

### | 2-4. バックアップ方法 (14.0%)

### | 2-5. 基本的な運用管理作業 (14.0%)

## || 3.【開発 / SQL】 (32.0%)
### | 3-1. SQLコマンド (26.0%)

### | 3-2. 組み込み関数 (4.0%)

### | 3-3. トランザクションの概念 (2.0%)

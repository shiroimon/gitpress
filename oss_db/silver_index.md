---
date   : 2021-09-29
title  : 【🥈Silver Ver.2.0】
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

#### ■出題項目・出題率
 |出題項目|出題率|
 |-|-|
 |【一般知識】|16.0%|
 |【運用管理】|52.0%|
 |【開発/SQL】|32.0%|

#### ■（公式）無料問題集+α
 + [Silverの例題解説「一般知識」 - OSS DB](https://oss-db.jp/sample/silver_knowledge_01)
 + [Silverの例題解説「運用管理」 - OSS DB](https://oss-db.jp/sample/silver_management_01)
 + [Silverの例題解説「開発/SQL」 - OSS DB](https://oss-db.jp/sample/silver_development_01)

#### ■OSS-DB silver - リファレンス
#### ■(Blog)
 + [OSS-DB Silver対策問題集](http://oss-db-taisaku.net/)

#### ■(Youtube)
 `OSS DB Exam Silver 技術解説セミナー`
 + [【Part1】試験の概要、PostgreSQLとは](https://www.youtube.com/watch?v=3iR6FiCSTKM) - SRA OSS, inc.
 + [【Part2】PostgreSQL 環境作り](https://www.youtube.com/watch?v=tG-8WBNxABI) - SRA OSS, inc.
 + [【Part3】PostgreSQL 基本操作 （起動/停止、ユーザ作成、データベース作成、psql）](https://www.youtube.com/watch?v=wSkZqw_IVps) - SRA OSS, inc.
 + [【Part4】PostgreSQL 設定ファイル（postgresql.conf）、Q&A](https://www.youtube.com/watch?v=QR0d0JTRwAk) - SRA OSS, inc.
 + [【Part5】PostgreSQL 設定ファイル（pg_hba.conf）、VACUUM/ANALYZE](https://www.youtube.com/watch?v=FKRXpuYJtPc) - SRA OSS, inc.
 + [【Part6】PostgreSQL バックアップ・リストア PITR ①](https://www.youtube.com/watch?v=Czx1Kr7OD-Q) - SRA OSS, inc.
 + [【Part7】PostgreSQL バックアップ・リストア PITR ②](https://www.youtube.com/watch?v=gP-ripEm4yo) - SRA OSS, inc.
 + [【Part8】Q&A](https://www.youtube.com/watch?v=WRAjfbHwJ04) - SRA OSS, inc.

 + [(2021/06/29)OSS-DB Exam Silver 技術解説セミナー](https://www.youtube.com/watch?v=_sPOt2_TMko) - OSSDB

#### ■PostgreSQL - リファレンス
#### ■(Blog)
 + [PostgreSQLの使い方](https://www.dbonline.jp/postgresql/) - DBOnline

#### ■(Youtube)
 **PostgreSQL再入門**
 + [【Part1】PostgreSQLの全体像](https://www.youtube.com/watch?v=zIH1dTNPL0A) - SRA OSS, inc.

#### ■合格体験記
 + [【勉強期間：2週間】OSS-DB Silverに合格するまで](https://marketingengineercareer.com/oss-db-silver) - 凡人データエンジニア
 +
 +

### | 戦略
 ![img](https://i.gyazo.com/81486c0db3a863f85f9883241f1bf3cd.png)
 より確度の高いエンジニアと名乗るには、全てを網羅している必要があると思う。
 しかし、試験通過を念頭とするので、試験の性質である出題割合の降順にしてみた。伴って、パワーバランスを明確にし取り組みたい。

## || 1.【一般知識】
### | 1-1. OSS -DBの一般的特徴 (8.0%)
 * [PostgreSQL 12.4文書](https://www.postgresql.jp/document/12/html/index.html) - PostgreSQLグローバル開発グループ

### | 1-2. リレーショナルデータベースに関する一般知識 (8.0%)

## || 2.【運用管理】
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
#### ■psql
    psqlはPostgreSQLに付属する対話型インターフェース。
    対話的にコマンドを入力し、SQL文やメタコマンドを実行。
    メタコマンドを使用することで、OSコマンドを実行することも可能。
    SQL文の実行は、psqlでPostgreSQLに接続してから行う方法と、-cオプションでSQL文を指定して接続時に行う方法がある。

     ・psqlではメタコマンドは1行以内。
     ・SQLは複数行記載可能。

    psql 内部で`！`書けばOSコマンドも使用可 `mydb=>  \! ls`

 + **psqlコマンド - SQLの実行**

  `psql {接続オプション} {オプション} {DB名/ユーザー名}`
     - `-c` psqlコマンドでDBに接続後に、直接(SQLを直書き)実行。
     - `-f` psqlコマンドでDBに接続後に、間接(SQLを書いたファイル指定)実行。

  |オプション|説明|
  |-|-|
  |`-l` `--list`|全てのDBのリストを表示した後、psql終了|
  |`-c {コマンド名}` `--command={コマンド名}`|指定したコマンドの実行結果を表示した後、psql終了|
  |`-f {ファイル名}` `--file={ファイル名}`|指定ファイルを読み込み、実行結果を表示した後、psql終了|
  |`-s` `--single-step`|各コマンド毎に実行するか否かの確認|
  |`-1` `--single-transaction`|複数のコマンドを1つにまとめて（トランザクション）実行 ※`-c` `-f`と併用|

 + **psqlコマンド - 接続**

  `psql {接続オプション} {オプション} {DB名/ユーザー名}`
  |接続オプション|説明|環境変数|デフォルト|
  |-|-|-|-|
  |`-U {ユーザー名}`|接続時のDBユーザー名指定|PGUSER|OSユーザー名|
  |`-h {ホスト名 or IPアドレス}`|TCP/IP UNIXドメインによる接続可能|PGHOST|UNIXドメイン|
  |`-d {DB名}`|接続先のDB指定|PGDATABASE|DBユーザー名|
  |`-p {Port番号}`|接続先ポート番号指定|PGPORT|5432|

 + **psql メタコマンド**
  |コマンド|取得する情報|
  |-|-|
  |`\l`|（一覧）DB|
  |`\dt`|（一覧）テーブル|
  |`\du`|（一覧）ユーザー|
  |`\d`|（一覧）テーブル、ビュー、シーケンス|
  |`\dp` `\z`|（一覧）テーブル、ビュー、シーケンス ※アクセス権限付|
  |`\?`|（一覧）メタコマンド|
  |`\!`|OSコマンドの実行結果|
  |`\h`|（一覧）SQLコマンドヘルプ|

#### ■pg _ctl コマンド
 `pg_ctl {サブコマンド} {オプション}`

 |サブコマンド|説明|
 |-|-|
 |`initdb` (init)|新規DBクラスタ作成|
 |`start`|バックグラウンドでPostgreSQLサーバー起動|
 |`stop`|シャットダウン|
 |`restart`|再起動（= stop→Start）|
 |`reload`|設定ファイル再読み込み|
 |`status`|PostgreSQLが稼働しているか確認|
 |`kill`|プロセスにシグナルを送信|

 + **pg _ctl コマンド - 再起動**

  `pg_ctl restart {オプション}`
  |オプション|説明|
  |-|-|
  |`-D {データベースクラスタ名}` `--pddata={データベースクラスタ名}`|DBクラスタ指定。（未指定は環境変数「$PGDATA」）|
  |`-m`|シャットダウンモード（3つの方式）|
  |`-W`|起動の完了を待たずにコマンド発行元に制御を戻す|
  |`-t {最大待ち時間}`|シャットダウン完了までの待ち時間指定（未指定60s）|

  `-m` シャットダウンモード
  ||||
  |-|-|-|
  |高速|`f` `fast`|実行中の処理を中断。強制的に接続を全て切断。（実行中のトランザクションはロールバック。実行前状態を維持。）|
  |即時|`i` `immediate`|実行中の処理を強制終了。（次回起動時に回復処理が走る。）|
  |スマート|`s` `smart`|全てのクライアント接続が切断後シャットダウン。（実行中トランザクションは処理されてからシャットダウン。）|

 + **pg _ctl コマンド - 再読み込み**

  `pg_ctl reload {オプション}`

  |オプション|説明|
  |-|-|
  |`-D {データベースクラスタ名}` `--pddata={データベースクラスタ名}`|DBクラスタ指定。（未指定は環境変数「$PGDATA」）|

 + **pg _ctl コマンド - プロセスにシグナルを送信**

  `pg_ctl kill {シグナル名} {プロセスID}`
  |シグナル名||説明|
  |-|-|-|
  |TERM|=`pg_ctl stop -m smart`|全待ちしてシャットダウン|
  |INT|=`pg_ctl stop -m fast`|高速シャットダウン|
  |QUIT|=`pg_ctl stop -m immediate`|即時シャットダウン|
  |HUP|=`pg_ctl reload`|設定ファイルを再読み込み|

#### ■ユーザー
 + **ユーザー作成**

  e.g. `createuser -U postgres --login --createdb sample`
       postgresユーザでデータベースに接続し、ログイン権限とデータベース作成権限を持つsampleユーザを作成
  `createuser {接続オプション} {オプション} {ユーザー名}`
  |オプション|説明|デフォルト|
  |-|-|-|
  |`-P` `--pwprompt`|パスワード設定||
  |`-s` `--superuser`|（許可）新規スーパーユーザー||
  |`-d` `--createdb`|（許可）新規DB作成||
  |`-r` `--createrole`|（許可）新規ロール作成||
  |`-l` `--login`|（許可）ログイン|●|
  |`-S` `--no-superuser`|（禁止）新規スーパーユーザー|●|
  |`-D` `--no-cratedb`|（禁止）新規DB作成|●|
  |`-R` `--no-createrole`|（禁止）新規ロール作成|●|
  |`-L` `--no-login`|（禁止）ログイン||

 + **ユーザー削除**

  `dropuser {接続オプション} {オプション} {ユーザー名}`
          e.g. dropuser -U postgres user
          e.g. dropuser -U postgres -i user
          ※ -i は削除前に確認促す（--interactive）

#### ■DB
 + **DB作成**

  `createdb {接続オプション} {オプション} {DB名}`
  |オプション|説明|
  |-|-|
  |`-E {エンコーディング}` `--encoding={エンコーディング}`|エンコーディング指定|
  |`-O {ユーザー名}` `--owner={ユーザー名}`|所有者指定|
  |`-l {ロケール名}` `--local={ロケール名}`|ロケール指定|
  |`-T {テンプレート名}` `--template={テンプレート名}`|テンプレート指定|

    * [template1] 基本的にはこっちをテンプレとして使われる。
    * [template0] エンコーディング、ロケールを指定したい場合はこっち。

 + **DB削除**

  `dropdb -U postgres foo`

  `DROP DATABASE foo;` (SQLコマンド)

### | 2-3. 設定ファイル (10.0%)
 + **postgresql.conf**
       postgresql.conf ファイルは PostgreSQL に関する基本的な設定を記述するファイル。
  ■ファイルの場所：
  ![img](https://i.gyazo.com/cb8f79dbe867e20e11439240e718d168.png)

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
    PostgreSQLにバックアップの開始・終了を通知する関数は、pg_start_backup()とpg_stop_backup()。
    PITRにおいてベースバックアップの取得を行う際に使用。

    (ベースバックアップの取得はPostgreSQLを稼働したままで行うことができる。）

    PostgreSQLの稼働中にバックアップを行う場合は、バックアップ中もデータが更新される可能性がある。
    そのため、バックアップ時にはバックアップ中に作成されたWALも自動でアーカイブされ、リカバリ時にWALアーカイブから更新内容が反映される。
    ベースバックアップはpg_basebackupコマンドを用いても取得できる。

 1. `SELECT pg_start_backup('ラベル名');` SELECT文でpg_start_backup()関数を呼び出す
 2. データベースクラスタ全体の物理バックアップを取得する
 3. `SELECT pg_stop_backup();` SELECT文でpg_stop_backup()関数を呼び出す


### | 2-5. 基本的な運用管理作業 (14.0%)

## || 3.【開発 / SQL】
### | 3-1. SQLコマンド (26.0%)

### | 3-2. 組み込み関数 (4.0%)

### | 3-3. トランザクションの概念 (2.0%)

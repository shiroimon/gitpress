---
date    : 2023-11-03
title   : ✴️ dbt
excerpt : 導入から実行まで
tags    : ["✴️", "dbt", "ETL/ELT", "MDS"]
---

## || dbt とは
> dbtとは `data build tool` の略で、データ統合を行う際のプロセスであるELT(抽出, 変換, 格納)のうち`Transform(変換)`の役割を担うツールです。
> Transformのプロセスでは一般的にデータウェアハウスなどに抽出したデータを下流の分析ツールやデータベースで利用できる形式に変換・加工する処理を行います。
> dbtはこの工程で役に立つ様々な機能を提供してくれます。
> 
> 引用先：[はじめて理解するdbt](https://www.isoroot.jp/blog/6054/) - isoroot 

`cf.`

|TITLE|URL|
|:-|:-|
|Official|https://www.getdbt.com/|
|Document|https://docs.getdbt.com/|
|GitHub|https://github.com/dbt-labs/dbt-core|
|dbt tokyo - Japan dbt User Group|https://dbt-ug.tokyo/|

    dbt-tokyoはdbtの日本におけるdbtの普及と,
    dbtプロダクトへの貢献を目的に活動をしています.

### | ChatGPTにも聞いてみた

🤖<「[・・・!!!](https://chat.openai.com/share/ef4f8048-2329-4dba-bde4-5f3b0161b8b6)」



## || 導入

    コンソール内の「#」は全てコメント.
    コンソールに直に記述するものではない.

### | STEP1 ✴️ dbt 導入
```shell
# 作業場用意
$ mkdir -p sandbox/dbt_training
$ cd dbt_training
#
# 仮想環境(venv)を用意
$ python3 -m venv venv
$ source venv/bin/activate        # 仮想環境を実行
(venv)$ pip install --upgrade pip # pip更新
(venv)$ pip install dbt-postgres  # dbtインストール
(venv)$ deactivate                # 仮想環境を停止 
$ 
#
# 再度仮想環境を実行
$ source venv/bin/activate
(venv)$　dbt --version # dbt環境が手元にあるか確認
# 必要なディレクトリ を準備
(venv)% mkdir models analysis tests seeds macros snapshots target
# dbt設定ファイルを作成（＊１: 後述のYAMLファイル）
(venv)% touch dbt_project.yml
```

`*1`: `/touch dbt_project.yml`.
```yaml
---
name: 'dbt_training'
config-version: 2
version: '1.0.0'
profile: 'dbt_training_dw'
model-paths: ["models"]
analysis-paths: ["analysis"]
test-paths: ["tests"]
seed-paths: ["seeds"]
macro-paths: ["macros"]
snapshot-paths: ["snapshots"]
target-path: "target"
clean-targets: [target, dbt_packages]
models:
    dbt_training:
         example:
```
YAMLファイルの中身の詳細説明は[ここ](https://zenn.dev/foursue/books/31456a86de5bb4/viewer/7fce02#%E5%90%84%E3%82%BF%E3%82%B0%E3%81%AE%E8%AA%AC%E6%98%8E)がわかりやすい.

```shell
# 上位階層に不可視ディレクトリ作成
$ mkdir .dbt
$ cd .dbt
# DWH接続ファイルを作成（＊２: 後述のYAMLファイル）
$ touch profiles.yml
```

`*2`: `~/.dbt/profiles.yml`.
```yaml
---
dbt_training_dw:
    target: dev
    outputs:
        dev:
            type: postgres
            host: localhost
            user: admin
            password: admin
            port: 5432
            dbname: postgres
            schema: public
            threads: 1
            keepalives_idle: 0 
            connect_timeout: 10
```
上記は「PostgreSQL」に接続させる設定ファイル（cf.[公式](https://docs.getdbt.com/docs/core/connect-data-platform/postgres-setup)）.
dbt はデータウェアハウス（DWH）の接続設定を `~/.dbt/profiles.yml` に書く.
また, `~/.dbt/profiles.yml` は各DWH毎にプロファイルを書く.
そのため, DWHの種類(PostgreSQL, Snowflake...etc.) 毎にアダプターがあり, プロファイルはアダプターごとに設定の書き方が異なる.


### | STEP2 🐘 DB（PostgreSQL）設定
`docker-compose.yml`を作成.
```yaml
---
version: '3'
services:
    postgres:
    image: postgres:latest
    restart: always
    ports:
        - 5432:5432
    environment:
        POSTGRES_USER: admin
        POSTGRES_PASSWORD: admin
    volumes:
        - ./postgres:/var/lib/postgresql/data
```

```shell
# データベース（PostgreSQL）を用意
(venv)$ touch docker-compose.yml
(venv)$ vim docker-compose.yml
#　 ↓
#　 #vimの説明は割愛
#　  [esc][I]押して、さっきのファイルの内容コピペ
#　  [esc][:wq]押して、抜ける。
#
(venv)$　docker --version # Dockerインストールされているか確認
# gemを新規で導入するときには、まず以下のコマンドを実行
# cf. https://qiita.com/KenAra/items/f1976caa69468323c29d -Qiita
(venv)$ docker-compose build
(venv)$ docker-compose up -d d # Docker起動
(venv)$ docker-compose stop    # Docker停止
```

もしここで`Docker`でつまいづいたら🐳 [コマンド参照](https://gitpress.io/c/docker_/mw_docker).


### | STEP3 ✴️ dbt 実行
```sell
(venv)$ dbt run
```
※モデル作成していないから, 「`WARNING`」出ているが気にせず.
逆に実行後 `logs/` ディレクトリができていることを確認.



## || REFERENCE
- [dbt 入門](https://zenn.dev/foursue/books/31456a86de5bb4) - Zenn
- [dbtで始めるデータパイプライン構築〜入門から実践〜](https://zenn.dev/dbt_tokyo/books/537de43829f3a0) - Zenn
- [はじめて理解するdbt](https://www.isoroot.jp/blog/6054/) - isoroot
- [データエンジニア界隈で話題のdbt（data build tool）のまとめ](https://qiita.com/manabian/items/67af7e4476d436aded77) - Qiita
- [What is dbt?](https://docs.getdbt.com/docs/introduction) - dbt
- [dbtとは？](https://zenn.dev/dbt_tokyo/books/537de43829f3a0/viewer/what_dbt) - Zenn
- [dbt プロジェクトの始め方](https://zenn.dev/foursue/books/31456a86de5bb4/viewer/04bca4) - Zenn (村長)
- [What Is DBT and Why You Should Use It?](https://www.aggua.io/blog/what-is-dbt-why-use-it) - aggua

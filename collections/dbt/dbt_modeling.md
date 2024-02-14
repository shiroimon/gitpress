---
date    : 2023-12-06
title   : ✴️ dbt Model作成 / 機能
excerpt : ---
tags    : ["✴️", "dbt", "ETL/ELT", "MDS"]
---

![image](https://github.com/sh16ma/gitpress/assets/150888300/d3b2fdbc-417f-4145-8f6b-d9e2ab58c483)

## || モデリング
### | Step1 データソースの準備
cf. 🐘 [PostgreSQL](https://gitpress.io/c/postgresql/sql_postgre)(コマンドリファレンス)
```shell
# PostgreSQLログイン
(venv)$ psql -U postgres # PostgreSQLログイン
postgres=#
# データベース（DB）作成
postgres=# \d # DB一覧確認
postgres=# CREATE DATABASE dbt_training; #
# テーブル作成
postgres=# \cd dbt_training # 作成したDBに移動
dbt_training=# CREATE SCHEMA raw; # スキーマ作成
dbt_training=# \du # ロール一覧確認
    List of schemas　
　Name      |  Owner
------------+----------
　public    | postgres
　raw       | postgres
    (2 rows)
# 各種テーブル作成
dbt_training=# 
-- 従業員テーブル
CREATE TABLE "dbt_training"."raw"."employees" (
	"employee_id" varchar(256),
	"first_name" varchar(256),
	"last_name" varchar(256),
	"email" varchar(256),
	"job_id" varchar(256),
	"loaded_at" timestamp
);
dbt_training=# 
-- お仕事テーブル
CREATE TABLE "dbt_training"."raw"."jobs" (
	"job_id" varchar(256),
	"job_title" varchar(256),
	"min_salary" INTEGER,
	"max_salary" INTEGER,
	"loaded_at" timestamp
);
dbt_training=# \dt myschema.* # スキーマ内テーブル一覧確認
# データ格納
dbt_training=# 
-- 従業員テーブルへデータをINSERT
INSERT INTO "dbt_training"."raw"."employees" VALUES
	('101','taro','yamada','yamada@example.com','11','2022-03-16'),
	('102','ziro','sato','satou@example.com','11','2022-03-16')
;
dbt_training=# 
-- お仕事テーブルへデータをINSERT
INSERT INTO "dbt_training"."raw"."jobs" VALUES
	('11','datascientist',6000000,12000000,'2022-03-16'),
	('12','dataengineer',5000000,10000000,'2022-03-16')
;
# PostgreSQLログアウト
dbt_training=# \q
postgres=# \q
```


### | Step2 モデル作成
cf. [SQL models](https://docs.getdbt.com/docs/build/sql-models) -dbt official

```shell
(venv)$ touch models/employee_names.sql)
```

```sql
-- employee_names.sql
select
	"employee_id"
	, concat("first_name", ' ', "last_name") as full_name
from
	"dbt_training"."raw"."employees"
```

```shell
(venv)$ dbt run 
…省略…
1 of 1 OK created view model public.employee_names.............................. [CREATE VIEW in 0.08s]
```
最終的に `1 of 1 OK created view model` と表示されればOK.
ココでは、デフォの`view`のモデルが作成される.
また, PostgreSQL の中に `employee_names` ビューが生成されているはず.

#### (補足) マテリアライゼーション
cf. [Materializations](https://docs.getdbt.com/docs/build/materializations) -dbt official

	マテリアライゼーション(Materializations)は dbt_project.yml か, 個別のモデル定義に直接書くことで設定できる.
	一般的には dbt_project.yml で書くほうが個別のモデルに書くよりも管理の面で有利.
	ただし, DWH によってはパフォーマンスのチューニングや特殊な設定を行いたい場合に, 個別のモデルに定義するほうが良い場合もある.

|種類|モデル|データ量|説明|
|:-|:-|:-|:-|
|`view`|View|-|未指定だとデフォ. 高速モデル構築（データ移動が発生しないから）. でも,`view < tablea`でクエリが遅い. 初期構築に向いている.|
|`table`|Tablea|小規模|実行(`dbt run`)の度にデータ入れ直す. その為小規模データに向いている（大規模データだと都度入れ直しはコストや処理時間面で問題）.参照回数の多い、アドホックな分析や集計後のモデリングに適している(`view < tablea`でクエリ速い為).|
|`incremental`|Tablea|大規模|実行(`dbt run`)の度にデータ入れ直す(新しいデータをInsertまたはUpdateで反映). `tablea < incremental`で速い. ただし,差分反映するための設定を追加で行う必要があり面倒.|
|`ephemeral`|SQL文の`CTE`に変換|-|DWH上では構築されず, ephemeralモデルは他のモデルから参照可. 再利用可能なモデルを作る場合に有効.|

### | Step3 モデル作成（マテリアライゼーション）
#### `Tablea` のモデル作成
YAMLファイルを修正してSQLファイル移動.
```yaml
# dbt_project.yml
---
…省略…
models:
  dbt_training:
    materialized_dwh:
      +materialized: table
```
```shell
(venv)$ mkdir models/materialized_dwh
(venv)$ mv models/employee_names.sql models/materialized_dwh/employee_names.sql
(venv)$ dbt run
…省略…
15:39:34  Finished running 1 table model in 0.34s.
15:39:34  
15:39:34  Completed successfully
15:39:34  
15:39:34  Done. PASS=1 WARN=0 ERROR=0 SKIP=0 TOTAL=1
```
View ではなく Tablea のモデルが作成されている.

#### `incremental` のモデル作成
cf. [Incremental models](https://docs.getdbt.com/docs/build/incremental-models) -dbt official

SQLファイルに直書き.
```sql
-- models/materialized_dwh/employee_names.sql
{{
    config(
        materialized='incremental'
    )
}}

select
	"employee_id"
	, concat("first_name", ' ', "last_name") as full_name
from
	"dbt_training"."raw"."employees"

{% if is_incremental() %}
where 
	"employee_id" not in (select "employee_id" from {{ this }})
{% endif %}
```

```shell
(venv)dbt_training % dbt run
…省略…
15:51:02  1 of 1 OK created incremental model public.employee_names....................... [INSERT 0 2 in 0.17s]
15:51:02  
15:51:02  Finished running 1 incremental model in 0.34s.
15:51:02  
15:51:02  Completed successfully
15:51:02  
15:51:02  Done. PASS=1 WARN=0 ERROR=0 SKIP=0 TOTAL=1
```
incremental モデルによって、INSERT が2行発生.
- ※ `where句` では, 既に追加済みの "employee_id" は含まれないようにフィルタリング(コレをしないと実行の度に差分更新で 都度追加=重複発生 させてしまう危険あり.)
- ※ `is_incremental()`はマクロで, モデルのマテリアライゼーションが incremental の場合に true 返す.
- ※ `{% if 条件 %} ~ {% endif %}` は, jinja のテンプレート機能で演算子を使える.
- ※ `{{ this }}`は, jinja のテンプレート機能で変数を使える.(モデル自身のテーブル名または, ビュー名)


## || モデルカスタム
### | エイリアス機能
cf. [Custom aliases](https://docs.getdbt.com/docs/build/custom-aliases)
```sql
-- hogehoge.sql
{{ config(alias='fugafuga') }}
```

モデル名(ビュー名)は`hogehoge`. 一方DBに作成されるテーブル名は`fugafuga`.

### | カスタムスキーマ機能
cf. [Custom schemas](https://docs.getdbt.com/docs/build/custom-schemas)

1. 利用シーン
```SQL
-- hogehoge.sql
{{ config(schema='piyopiyo') }}
```
このように作られる=> `public_piyopiyo`

2. 利用シーン
```SQL
-- hogehoge.sql
{{ config(alias='fugafuga', schema='piyopiyo') }}
```
このように作られる=> `public_piyopiyo.fugafuga`

3. 利用シーン
```yaml
---
models:
  hoge_project:
    piyo:
      +schema: piyopiyo
```

`models/piyo` ディレクトリ配下のモデルは `<target_shema>_piyopiyo` スキーマに保存できる.

### | カスタムデータベース機能
cf. [Custom databases](https://docs.getdbt.com/docs/build/custom-databases)
### | 変数機能
cf. [Project variables](https://docs.getdbt.com/docs/build/project-variables)




## || REFERENCE 
- [モデルを作ろう](https://zenn.dev/foursue/books/31456a86de5bb4/viewer/6037e5) -Zenn
- [モデルをもう少し深く使おう](https://zenn.dev/foursue/books/31456a86de5bb4/viewer/4201af#%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%99%E3%82%8B%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%A0%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E6%A9%9F%E8%83%BD) -Zenn



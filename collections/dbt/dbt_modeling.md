---
date    : 2023-12-06
title   : ✴️ dbt
excerpt : モデル作成からモデリング
tags    : ["✴️", "dbt", "ETL/ELT", "MDS"]
---

## || モデリング

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
🐘[PostgreSQL](https://gitpress.io/c/postgresql/sql_postgre)のコマンド参照。



## || REFERENCE 
- []() -
-

---
date   : 2022-03-11
title  : INFORMATION_SCHEMA
excerpt: 
tags   : ["Google BigQuery", "SQL", "INFORMATION_SCHEMA"]
---

## || INFORMATION_SCHEMA 
> BigQuery INFORMATION_SCHEMA ビューは、BigQuery オブジェクトに関するメタデータ情報を提供するシステム定義の読み取り専用ビューです。
>
>『 [BigQuery INFORMATION_SCHEMA の概要](https://cloud.google.com/bigquery/docs/information-schema-intro?hl=ja)』より



## || テーブル確認（カラム）
```SQL
select
    table_name
    , column_name
    , data_type
from 
    `my_pj.my_ds.INFORMATION_SCHEMA.COLUMNS`
where 
    table_name = "yellow_taxi_2015"
;

/*
| |table_name      |column_name     |data_type|
|-|-               |-               |-        |
|1|yellow_taxi_2015|vendor_id       |STRING   |
|2|yellow_taxi_2015|pickup_datetime |TIMESTAMP|
|3|yellow_taxi_2015|dropoff_datetime|TIMESTAMP|
*/
```



## || NFORMATION_SCHEMA.JOBS_BY_PROJECT
● クエリ世代確認等にも使える。
```SQL
SELECT
    ROW_NUMBER() OVER(PARTITION BY job_type ORDER BY creation_time) AS number
    , creation_time
    , user_email
    , job_type
    , start_time
    , end_time
    , query
    , state
FROM 
    `region-us`.INFORMATION_SCHEMA.JOBS_BY_PROJECT
WHERE 
        query LIKE '%TAB3_%'
    AND query NOT LIKE '%T_OWN_EXPENSE_TAB3_SS_CV%'
    AND DATE(start_time) BETWEEN '2022-07-01' AND '2022-07-31'
ORDER BY 
    start_time
```
インシデント；
あるプロジェクトで、クエリ自体が紛失してしまった。
そこで、過去にBigQueryで走らせたクエリをログベースで確認することができる。



## || REFERENCE
+ [BigQuery INFORMATION_SCHEMA の概要](https://cloud.google.com/bigquery/docs/information-schema-intro?hl=ja) - Google Cloud
+ [BigQuery で INFORMATION_SCHEMA から CREATE TABLE 文が取得できるようになりました！](https://dev.classmethod.jp/articles/bigquery-information-schema-get-create-table-ddl/) - DevelopersIO
+ [BigQuery の INFORMATION_SCHEMA からどんな情報が取得できるのか、全ての VIEW を確認してみた](https://dev.classmethod.jp/articles/bigquery-information-schema-view-all/) - DevelopersIO
+ [INFORMATION_SCHEMAでBigQueryの利用状況を確認](https://www.niandc.co.jp/sol/tech/date20200923_1893.php) - PlanB
+ [BigQueryのメタ情報を見てみた＆使ってみた](https://qiita.com/CraveOwl/items/809b70f2c49c28012f2a) - Qiita
+ [[bigquery]特定のカラム名を持っているテーブル情報を探す方法](https://apl-py.com/tec/bigquery%E7%89%B9%E5%AE%9A%E3%81%AE%E3%82%AB%E3%83%A9%E3%83%A0%E5%90%8D%E3%82%92%E6%8C%81%E3%81%A3%E3%81%A6%E3%81%84%E3%82%8B%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E6%83%85%E5%A0%B1%E3%82%92%E6%8E%A2) - 目黒で働く分析担当の作業メモ

---
date    : 2021-10-26
title   : 【🔎 SQL】テーブル確認
excerpt : SQLでデータを分析するにあたり基礎的なウォーミングアップ。
tags    : ["SQL", "BigQuery", "レコード数", "カラム数"]
---

## || テーブル確認（全体）
```SQL
select
    *
from 'my_pj.my_ds.yellow_taxi_2015'
;
```
| |vendor_id|pickup_datetime|dropoff_datetime|passenger_count|payment_type|trip_distance|pickup_longitude|pickup_latitude|rate_code|store_and_fwd_flag|
|-|-|-|-|-|-|-|-|-|-|-|
|CMT|2015-12-02T18:40:42Z|2015-12-02T18:55:58Z|1|1|2.0|-73.984215|40.77003|1.0|0.0|-73.9625|


## || テーブル確認（レコード数）
```SQL
select
    count(*) as record
from 'my_pj.my_ds.yellow_taxi_2015'
;
```
| |record     |
|-|-          |
|1|146,112,991|


## || テーブル確認（カラム）
```SQL
select
    table_name,
    column_name,
    data_type
from `my_pj.my_ds.INFORMATION_SCHEMA.COLUMNS`
where table_name="yellow_taxi_2015"
;
```
| |table_name      |column_name     |data_type|
|-|-               |-               |-        |
|1|yellow_taxi_2015|vendor_id       |STRING   |
|2|yellow_taxi_2015|pickup_datetime |TIMESTAMP|
|3|yellow_taxi_2015|dropoff_datetime|TIMESTAMP|

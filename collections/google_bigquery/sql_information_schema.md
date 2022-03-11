---
date   : 2022-03-11
title  : INFORMATION_SCHEMA
excerpt: 
tags   : ["Google BigQuery", "SQL", "INFORMATION_SCHEMA"]
---
## || INFORMATION_SCHEMA 


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

## || Cf.
+ [BigQuery で INFORMATION_SCHEMA から CREATE TABLE 文が取得できるようになりました！](https://dev.classmethod.jp/articles/bigquery-information-schema-get-create-table-ddl/) - DevelopersIO

---
date    : 2021-10-25
title   : 【🔎 SQL】繰り返し処理できるの？ - BigQueryScripting
excerpt : BigQueryでSQLを書いていて繰り返し処理できたらいいのにと思っているのです。
tags    : ["SQL", "BigQuery", "BigQueryScripting", "β"]
---
## || LOOP処理
```SQL
DECLARE x INT64 DEFAULT 0;
LOOP
  SET x = x + 1;
  IF x >= 10 THEN
    LEAVE;
  END IF;
END LOOP;
SELECT x;
```

## || BigQueryScriptingってなに？
```SQL
-- Declare a variable to hold names as an array.
DECLARE top_names ARRAY<STRING>;
-- Build an array of the top 100 names from the year 2017.
SET top_names = (
  SELECT ARRAY_AGG(name ORDER BY number DESC LIMIT 100)
  FROM `bigquery-public-data`.usa_names.usa_1910_current
  WHERE year = 2017
);
-- Which names appear as words in Shakespeare's plays?
SELECT
  name AS shakespeare_name
FROM UNNEST(top_names) AS name
WHERE name IN (
  SELECT word
  FROM `bigquery-public-data`.samples.shakespeare
);
```
Cf.
+ [BigQuery ScriptingがBetaリリースされたので軽くウォークスルーしてみる
](https://medium.com/google-cloud-jp/bigquery-scripting%E3%81%8Cbeta%E3%83%AA%E3%83%AA%E3%83%BC%E3%82%B9%E3%81%95%E3%82%8C%E3%81%9F%E3%81%AE%E3%81%A7%E8%BB%BD%E3%81%8F%E3%82%A6%E3%82%A9%E3%83%BC%E3%82%AF%E3%82%B9%E3%83%AB%E3%83%BC%E3%81%97%E3%81%A6%E3%81%BF%E3%82%8B-1408bab2c026) - medium
+ [Release notes](https://cloud.google.com/bigquery/docs/release-notes) - GoogleCloud
+ [標準 SQL のスクリプト ステートメント](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting) - GoogleCloud

## || 早速やってみよう！

結論、まだできね (><;)

（ちょっとづつ完成に向けて試行錯誤するか...）

```SQL
DECLARE x INT64 DEFAULT 1;
DECLARE column_name ARRAY<STRING>;
DECLARE result ARRAY<STRING>;

SET column_name = [
    'vendor_id', 'pickup_datetime','dropoff_datetime', 'passenger_count',
    'payment_type', 'trip_distance', 'pickup_longitude', 'pickup_latitude',
    'rate_code', 'store_and_fwd_flag', 'dropoff_longitude', 'dropoff_latitude', 	
    'fare_amount', 'surcharge', 'mta_tax', 'tip_amount', 'tolls_amount', 'total_amount'];

SET result = [];

WHILE x < array_length(column_name) DO
    select
        count(column_name) as n,
        avg(column_name) as mean,
        stddev(column_name) as std,
        min(column_name) as min,
        (select q from(select percentile_cont(column_name, 0.25) over() as q from `pj.ds.tb`) group by q) as first_quartile,
        (select q from(select percentile_cont(column_name, 0.5) over() as q from `pj.ds.tb`) group by q) as median,
        (select q from(select percentile_cont(column_name, 0.75) over() as q from `pj.ds.tb`) group by q) as thrd_quartile,
        max(column_name) as max
    from `pj.ds.tb`
    where passenger_count is not null;

    SET x = x + 1;

END WHILE;
select x;
```

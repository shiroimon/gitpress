---
date    : 2023-09-21
title   : 🔍 ARRAY_AGG関数
excerpt : ---
tags    : ["Google BigQuery", "array_agg"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || array_agg()
### | 基本
```sql
#standardSQL
select
    user_id
    , ARRAY_AGG(access_pagepath) as pagepath
from
    `{project名}>.{dataset名}.array_agg_test`
group by
    user_id
```

|user_id|access_domain|access_pagepath|
|:-:|:-|:-|
|1|test.co.jp|/page1|
|1|test.co.jp|/page2|
|2|test.co.jp|/dir1|
|3|test.co.jp|/page1|
|3|test.co.jp|/dir1|

cf.【GCP】BigQueryのARRAY_AGG関数 

　**↓**

|user_id|access_pagepath|
|:-|:-|
|1|[/page1,page2]|
|2|[/dir1]
|3|[/page1,/dir1]|

cf.【GCP】BigQueryのARRAY_AGG関数 

e.g 

### | JS利用
```sql
#standardSQL
--UDF関数（配列データを1つの文字列に変換する--
CREATE TEMP FUNCTION concatString(pathList ARRAY<string>)
   RETURNS string
   LANGUAGE js as
   """
     var str = "";
     for(element of pathList){
       str += element;
     }
     return str;
   """;

select
    user_id
    , concatString(ARRAY_AGG(access_pagepath)) as pagepath
from `firebase-test.bigquery_test.array_agg_test`
group by user_id
;
```


## || REFERENCE
- [ARRAY_AGG](https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions#array_agg) - GoogleCloud
- [9.20. 集約関数](https://www.postgresql.jp/document/9.6/html/functions-aggregate.html) - PostgreSQL 9.6.5文書
- [Big QueryでWindow関数を用いて、累積和を計算する](https://ex-ture.com/blog/2019/09/04/bigquery_window_sum/) - エクスチュア株式会社ブログ
- [【GCP】BigQueryのARRAY_AGG関数](https://yosshiblog.jp/gcp_bigquery-arrayagg/) - Yosshi Labo.


---
date    : 2023-09-21
title   : 🔍 ARRAY_AGG関数
excerpt : ---
tags    : ["Google BigQuery", "array_agg"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || array_agg()
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

#### コレをコウ

|user_id|access_domain|access_pagepath|
|:-:|:-|:-|
|1|test.co.jp|/page1|
|1|test.co.jp|/page2|
|2|test.co.jp|/dir1|
|3|test.co.jp|/page1|
|3|test.co.jp|/dir1|

　**↓**

|user_id|access_pagepath|
|:-|:-|
|1|[/page1,page2]|
|2|[/dir1]
|3|[/page1,/dir1]|


`cf.`
- [ARRAY_AGG](https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions#array_agg) - GoogleCloud
- [9.20. 集約関数](https://www.postgresql.jp/document/9.6/html/functions-aggregate.html) - PostgreSQL 9.6.5文書
- [Big QueryでWindow関数を用いて、累積和を計算する](https://ex-ture.com/blog/2019/09/04/bigquery_window_sum/) - エクスチュア株式会社ブログ
- [【GCP】BigQueryのARRAY_AGG関数](https://yosshiblog.jp/gcp_bigquery-arrayagg/) - Yosshi Labo.

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
group by 1
;
```

### | array_agg struct
```sql
#standardSQL
~CTE中略~

#output
    -- Slack通知用テーブル
    , slack_notification as (
        select
            user_email
            , array_agg(
                struct<
                    job_id string
                    , project_id string
                    -- , destination_path string
                >(
                    job_id
                    , project_id
                    -- , destination_path
                )
              ) as execution_jobs
            , any_value(collect_project_id) as collect_project_id
            , any_value(team_path) as team_path
            , any_value(is_belong_to_engineerteam) as is_belong_to_engineerteam
        from
            rawdata
        group by 1
    )

select * from slack_notification 
-- limit 1 --（検証時に追記）
;
```

`cf.`
- []() -

### | array_agg in array_agg 
```sql
#standardSQL
~ CTE中略 ~

#output

    --▼Slack通知： 各所属部署別×（所属チームメンバー別×実行ジョブ情報）一覧
    , slack_notification as (
        select
            team_path_for_each_project as path
            , array_agg((user_email, execution_jobs)) as notice
        from (
            select
                team_path_for_each_project
                , user_email
                , array_agg(struct<
                        project_id string, job_id string, query_type string, url string, total_slot_hour int64
                    >(
                        project_id, job_id, query_type, url, total_slot_hour
                    )
                ) as execution_jobs
            from rawdata
            group by 1, 2
        )
        group by 1
    )

select * from slack_notification
;

/*****
 | |path     |notice._field_1  |notice._field_2.project_id|~|notice._field_2.total_slot_hour|
 |1|Engineer |hogeyama@piyo.com|production                |~|                 83495372400000|
 | |         |                 |dev_ci                    |~|                 72495372300000|
 | |         |hogesaki@piyo.com|prodaction                |~|                  9495372400000|
 |2|Marketing|fuga@piyo.com    |analize                   |~|                183495372400000|
 *****/
```

1. 子サブクエリでarray_aggでグルーピング
2. 親サブクエリでarray_aggでグルーピング（タプル） 

`cf.`
- [is it possible to nest an array_agg inside another array_agg](https://stackoverflow.com/questions/57463794/is-it-possible-to-nest-an-array-agg-inside-another-array-agg) - stackoverflow



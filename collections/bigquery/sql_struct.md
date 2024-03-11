---
date   : 2022-05-14
title  : 🔍 STRUCT関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "struct"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || struct()


## || array struct での集計
▼こんなCTEテーブル作って。
```sql
~
-- Slack通知用テーブル
, slack_notification as (
    select
        user_email
        , array_agg(
              struct
                  <job_id string, project_id string>
                  (job_id, project_id)
          ) as execution_jobs
        , any_value(collect_project_id) as c_project_id
        , any_value(team_path) as team_path
    from
        rawdata
    group by 1
)
/*****
|Row|user_email    |execution_jobs.job_id|execution_jobs.project_id|c_project_id|team_path|
|1  |fuga@test.com |bquxjob_123456       |pj_120                   |pj_001      |engineer |
|2  |              |scripts_123456       |pj_300                   |            |         |
|3  |              |bquxjob_523457       |pj_300                   |            |         |
|4  |              |bquxjob_765849       |pj_560                   |            |         |
|5  |              |scripts_765432       |pj_460                   |            |marketing|
~
|19 |hoge@test.com |sheets_123456        |pj_670                   |Pj_003      |         |
|20 |              |bquxjob_654321       |pj_100                   |            |         |
 *****/
```
こんな集計したい（けどデキナイ...）。
```sql
select 
    user_email
    , c_project_id
    , count(distinct execution_jobs.job_id) as job_count
from 
    slack_notification
group by 
    1, 2 
```
ので、こうしたらデキる。
```sql
select
    user_email
    , c_project_id
    , (select as struct count(*) as count from unnest(execution_jobs) as execution_jobs) as job_count
from 
    slack_notification
/*****
|Row|user_email   |c_project_id|job_count.count|
|1  |fuga@test.com|pj_001      |             18|
|2  |hoge@test.com|pj_003      |            172|
 *****/
```

`cf.`
- [How to count frequency of elements in a bigquery array field](https://stackoverflow.com/questions/48411331/how-to-count-frequency-of-elements-in-a-bigquery-array-field) - Stackoverflow



## || cf.
- [BigQueryのARRAYとSTRUCTを理解して使いこなす](https://blog.g-gen.co.jp/entry/array-and-struct-type-in-bigquery) -Ggen
- [GA4データの複雑な構造に立ち向かう(BigQuery:ARRAY,STRUCT)](https://zenn.dev/sql_geinin/articles/20176241442f7d) -Zenn 




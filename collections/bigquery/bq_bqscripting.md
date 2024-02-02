---
date    : 2022-01-01
title   : BigQuery Scripting
excerpt : 
tags    : ["Google BigQuery", ""]
---
## || BigQuery Scripting

### | 変数宣言
```SQL
# 集計期間
decleare TERM_START string default '2020-01-01';
decleare TERM_END   string default '2020-12-31';
decleare PREP_TS string;

set PREP_TS = (
    select format_date('%Y%m%d', current_date('Asia/Tokyo')) as TODAY
);

with
    , rsv as (
        select * from `pj.ds.hoge_reservation_*` where _TABLE_SUFFIX = PREP_TABLE_SUFFIX
    )
    , output as (
        select
            MENU_ID
            , count(distinct LOG_NO) as CV
        from 
            rsv
        where
            RSV_DATE_TRUNC between datetime(TERM_START) and datetime(TERM_END)
    )
select * from output;
```

cf.[手続き型言語](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting)※1



## || Reference
+ [手続き型言語](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting) - GoogleCloud (※1)
+ [BigQuery ScriptingでPythonっぽいループ処理をしてみた](https://qiita.com/CraveOwl/items/5ffcf5edac238b165bbb) - Qiita
+ [BigQuery ScriptingがBetaリリースされたので軽くウォークスルーしてみる](https://medium.com/google-cloud-jp/bigquery-scripting%E3%81%8Cbeta%E3%83%AA%E3%83%AA%E3%83%BC%E3%82%B9%E3%81%95%E3%82%8C%E3%81%9F%E3%81%AE%E3%81%A7%E8%BB%BD%E3%81%8F%E3%82%A6%E3%82%A9%E3%83%BC%E3%82%AF%E3%82%B9%E3%83%AB%E3%83%BC%E3%81%97%E3%81%A6%E3%81%BF%E3%82%8B-1408bab2c026) - medium
https://www.yasuhisay.info/entry/2022/03/14/093500
+ [BigQuery Scriptingの便利な使い方をまとめてみた](https://www.yasuhisay.info/entry/2022/03/14/093500) - yasuhisa's blog

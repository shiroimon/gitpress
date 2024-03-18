---
date    : 2024-01-01
title   : 🔍 UTC / JST
excerpt : ---
tags    : ["🔍", "BigQuery", "GoogleCloud", "utc", "jst"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || UTC / JST
### | MOTIVATION

知っているようで知らない。（知らんけんど）

### | UTC 
🇬🇧UTCはイギリス時間。
🇯🇵JSTは日本時間。
UTCは、協定世界時間と一致する標準時のこと。
日本とは-9時間のズレがある。
（e.g. 日本午前9時になると現地時間と同じ日付になる）


### | 調査方法
```sql
select 
    timestamp_trunc(created, hour) as h --「created」がそのまま使える状態（JST）なのか？ 
    , count(id) --時間で「group by」しているので、1時間ごとのListing数を観測
from 
    `pj`,`ds`,`items_table`
where 
    date(created) = date('2024-03-03') --任意の特定日で十分！ 
group by h
order by h
```
抽出した数字を ~泥臭く~ 観測することで、分布的に「9時間のズレ」がアリそうかわかる。



## || REFERENCE
- [SQLオンボーディング講座 JP](https://mercari.atlassian.net/wiki/spaces/MJP/pages/1315900440/SQL+JP) -MERCARI
- []() -





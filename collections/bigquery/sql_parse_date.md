---
date    : 2021-09-24
title   : 🔍 PARSE_DATE関数
excerpt : ---
tags    : ["Google BigQuery", "SQL", "parse_date", "日付変換"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || parse_date()

文字型（STRING）の日付データを日付型（DATE）にしたい時に使える。

```sql
select
    parse_date('%Y %m %d', '20180101') -- 2018-01-01に変換される
```



## || cf.
+ [PARSE_DATE](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions?hl=ja#parse_date) - GoogleCloud
+ [BigQueryでstring型の文字列からdate型の日付に変換する](https://ten-ezo.com/62ac07f58f3d46f08e1eee524476acd8) - TEN's life
+ [BigQueryでSTRING型の日付データ（YYYYMMDDの文字列）を日付型に変換する方法](https://note.com/mignon53/n/n6b923099787e) - note

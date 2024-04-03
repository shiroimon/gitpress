---
date    : 2024-01-01
title   : 🔍 BigQueryのDate
excerpt : ---
tags    : ["🔍", "BigQuery", "GoogleCloud"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || 様々なDATE
### | MOTIVATION

割と舐めがちな`DATE` (いや、俺だけか...) <br>
舐めてると大怪我するので、覚書しておいた方がいいと記述。


### | 日時、週次、月次、年次（集計）

大体extractでなんとかなる。

- 週次
```sql
select
    extract(week from date(create_datetime, 'Asia/Tokyo')) as  week_num
    , count(amount) as amount_ttl
from
    {table}
group by 1
;
```
- 月次
```sql
select
    extract(year from date(create_datetime, 'Asia/Tokyo')) as year
    , extract(month from date(create_datetime, 'Asia/Tokyo')) as month
    , count(amount) as amount_ttl
from
    {table}
group by 1, 2
;
```


### | 集計規定

    （e.g 日次とか）集計中にnull で丸まっちゃう時、レコードを保持させたい時に使う

```sql
monthly_series as (
  select
    monthly
  from
    unnest(GENERATE_DATE_ARRAY(start_month, end_manth) as monthly
)
```



## || REFERENCE
- [Great Google BigQuery Date Functions to Know](https://coffingdw.com/great-google-bigquery-date-functions-to-know/) -Coffing
- [【SQL】BigQueryにおける DATE_TRUNC関数とは？使い方をわかりやすく解説！](https://programmingnote.jp/archives/2186) -独学プログラマーのプログラミングノート
- [【基本】単一のフィールド（例. 日付）をキーとして歯抜けとなったデータを保管する](https://zenn.dev/cureapp/articles/bigquery-monthly-series) -Zenn
- [BigQueryで日次、週次、月次の集計をする](https://qiita.com/akinov/items/845961a6d1dc5d8843ca) -Qiita




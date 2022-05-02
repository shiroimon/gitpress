---
date   : 2022-03-02
title  : unnest()
excerpt: UNNEST関数
tags   : ["Google BigQuery", "SQL", "unnest()"]
---

## | UNNEST()

UNNEST を使うと、ARRAYなどの配列や、REPEATEDなカラムを、開くことができる。

```sql
```

```sql 
    # unnest を用いて、親テーブルに無い値を付与
    , search_list_marge as (
        select distinct KEY_CODE, KEY_NAME from `project.datasets.search_table_20*` where _TABLE_SUFFIX = (select TODAY from ts)
        union all
        select * from
            unnest(
                array
                <struct< KEY_CODE STRING, KEY_NAME STRING>>
                [("reserve_tomorrow","明日予約可"), ("reserve_today","今日予約可")]
            )
    )
```


## | cf.
+ [BigqueryでUNNESTを使いこなせ！クエリ効率１００%！！最強！！](https://medium.com/eureka-engineering/bigquery-unnest-100percent-3d28560b4f0)-  medium
+ [BigQuery 活用術: UNNEST 関数](https://developers-jp.googleblog.com/2017/04/bigquery-tip-unnest-function.html)- Google Developers
+ [BigQuery で複数の配列をフラット化する](https://labs.septeni.co.jp/entry/2018/11/06/120000) - FLINTERS Engineer's Blog

+ [BigQuery での JSON、配列、構造体の操作](https://www.cloudskillsboost.google/focuses/3696?locale=ja&parent=catalog) -　Google Cloud
+ [BigQueryで1列に格納されている配列を別々の列に分解したい](https://teratail.com/questions/151185) - teratail 

+ [BigQueryのArrayを理解する。](https://zenn.dev/a1008u/articles/acbd17961f7f5d95a2a8) - Zenn
+ [GoogleAnalytics 4 のイベントを BigQuery で集計する](https://zenn.dev/mjunya1030/articles/20210510-analyze-ga4-by-bigquery) - Zenn
+ [BigQuery で実行できる SQL と実行できない SQL](https://dev.classmethod.jp/articles/bigquery-standard-sql-support/) - DevelopersIO
+ []() - 

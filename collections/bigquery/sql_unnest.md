---
date   : 2022-03-02
title  : 🔍 UNNEST関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "unnest()"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || unnest()

[配列型](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types?hl=ja#array_type)
|名前|説明|
|:-:|:-:|
|ARRAY|ARRAY 型ではないゼロ以上の要素の順序付きリスト。|

UNNEST を使うと、ARRAYなどの配列や、[REPEATEDなカラム](https://itips.krsw.biz/bigquery-how-to-count-repeated-column-record/)を、開くことができる。

```sql
select 
    fullVisitorId as UU
    , visitNumber as SS
    , struct (
          h.eventInfo.eventCategory as CATEGORY
        , h.eventInfo.eventAction as ACTION
        , h.eventInfo.eventLabel as LABEL
        , h.eventInfo.eventValue as VALUE
      ) as EVENT
    , ht.customDimensions as CUSTOM_DIMENSIONS_HITS
    , ht.experiment as EXPERIMENT
from 
    ga , unnest(hits) as ht

-- from ga
-- cross join unnest(hits) as ht
```
上記のクエリでは「カンマ演算子（`,`）」で「CROSSJOIN」を暗黙的に実行されている。
（cf.[配列操作](https://cloud.google.com/bigquery/docs/reference/standard-sql/arrays?hl=ja) - GoogleCloud）


### | e.g.
`ARRAY<type>[]`
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

```SQL
#standardSQL
SELECT 
    event, event.name, event.timestamp_micros
FROM 
    `firebase-analytics-sample-data.android_dataset.app_events_20160607`
    , UNNEST(event_dim) as event
    , UNNEST(event.params) as event_param
WHERE 
        event.name = "round_completed"
    AND event_param.key = "score"
    AND 10000 < event_param.value.int_value
```

```SQL
#standardSQL
with
    sequences as (
        select  [
            'あ','い','う','え','お',
            'か','き','く','け','こ',
            'さ','し','す','せ','そ',
            'た','ち','つ','て','と',
            'な','に','ぬ','ね','の',
            'は','ひ','ふ','へ','ほ',
            'ま','み','む','め','も',
            'や','ゆ','よ',
            'ら','り','る','れ','ろ',
            'わ','を','ん'] AS n
    )

# 2単語の組み合わせ
select
      na1 as firstL
    , na2 as lastL
    , concat(na1, na2) as name
from 
    sequences
    , unnest(sequences.n) as na1
    , unnest(sequences.n) as na2
where
      na1 not in ('を','ん')
  and na1 != na2
;

# 3単語の組み合わせ
select 
--     na1 as firstL
--   , na2 as secondL
--   , na3 as lastL
--   , concat(na1, na2, na3) as name
-- from
--   sequences
--   , unnest(sequences.n) as na1
--   , unnest(sequences.n) as na2
--   , unnest(sequences.n) as na3
-- where
--       na1 not in ('を','ん')
--   and na1 != na2
--   and na2 != na3
-- ;
```
「ABC分析」「アソシエーション分析」等にも応用できそう。（[商品分析の手法（ABC分析、アソシエーション分析）](https://www.albert2005.co.jp/knowledge/marketing/customer_product_analysis/abc_association) - Albeart,
[どの単語の組み合わせがよく使われるかを分析する方法](https://exploratory.io/note/GMq1Qom5tS/FVI2obS0jW) - Exporatory)


## || REFERENCE
+ [テーブル スキーマでネストされた列と繰り返し列を指定する](https://cloud.google.com/bigquery/docs/nested-repeated?hl=ja) - GoogleCloud
+ [BigQueryでrepeated型カラムの行数を数える方法](https://itips.krsw.biz/bigquery-how-to-count-repeated-column-record/) - ITips
+ [BigqueryでUNNESTを使いこなせ！クエリ効率１００%！！最強！！](https://medium.com/eureka-engineering/bigquery-unnest-100percent-3d28560b4f0)-  medium
+ [BigQuery 活用術: UNNEST 関数](https://developers-jp.googleblog.com/2017/04/bigquery-tip-unnest-function.html)- Google Developers
+ [BigQuery で複数の配列をフラット化する](https://labs.septeni.co.jp/entry/2018/11/06/120000) - FLINTERS Engineer's Blog

+ [BigQuery での JSON、配列、構造体の操作](https://www.cloudskillsboost.google/focuses/3696?locale=ja&parent=catalog) -　Google Cloud
+ [BigQueryで1列に格納されている配列を別々の列に分解したい](https://teratail.com/questions/151185) - teratail 

+ [BigQueryのArrayを理解する。](https://zenn.dev/a1008u/articles/acbd17961f7f5d95a2a8) - Zenn
+ [GoogleAnalytics 4 のイベントを BigQuery で集計する](https://zenn.dev/mjunya1030/articles/20210510-analyze-ga4-by-bigquery) - Zenn
+ [BigQuery で実行できる SQL と実行できない SQL](https://dev.classmethod.jp/articles/bigquery-standard-sql-support/) - DevelopersIO

---
date   : 2022-02-25
title  : 🔍 COALESCE関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "分析", "coalesce"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || coalesce（）

* 欠損処理に使える。(Null補完ができる。)

```sql
select 
    ID
    , coalesce(height, avg(mean)) as HEIGHT
    , coalesce(weight, avg(mean)) as WEIGHT 
from 
    BMI
```
身長体重をユーザー別に記録しテーブルがあったとして、「NulL」のデータを全体の平均値で埋めたい時。とか


* 抽出時の条件文的にも使える。

```sql
select disinct
    r.MEMBER_ID
    , coalesce(m.INTEGRATION_ID1, D.INTEGRATION_ID_1, CAST(P.STATION_ID AS STRING)) AS STATION_ID 
    , coalesce(m.STATION1, D.STATION1, P.STATION_NAME) AS STATION_NAME
    , coalesce(
        concat(m.PROV_NAME, '_', REPLACE(m.STATION1,'駅', ''))
        , concat(d.PROV_NAME, '_', REPLACE(d.STATION1,'駅', ''))
        , concat(p.PROV_NAME, '_', REPLACE(p.STATION_NAME,'駅', ''))
    ) AS KEY
from 
    T_RSV_PREV as r
left outer join
    M_MEDICAL_ALL as m
on
    r.SHOPO_ID = m.SHOP_ID
    and r.TYPE_CODE = 'M'
left outer join
    M_DENTAL_ALL as d
on
    r.SHOP_ID = d.SHOP_ID
    and r.TYPE_CODE = 'D'
left outer join 
    M_PHARMACY_ALL as p
on
    r.SHOP_ID = CAST(p.shop_id AS STRING)
    and r.TYPE_CODE = 'P'
```

あるユーザーに対して、業種別のテーブルをマージして、「Null」でない値を取得する場合。とか



## || cf. 
+ [COALESCE Function](https://docs.trifacta.com/display/DP/COALESCE+Function) - 
+ [SQL関数coalesceの使い方と読み方](https://spirits.appirits.com/doruby/8666/) - 
+ [COALESCE関数](https://e-words.jp/w/COALESCE%E9%96%A2%E6%95%B0.html) -

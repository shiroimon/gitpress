---
date   : 2022-03-2
title  : 🔍 ST_DISTANCE関数
excerpt: 緯度・経度を用いて分析したい時に使える関数
tags   : ["Google BigQuery", "SQL", "地理関数"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || st_distance()

緯度・経度を用いて、位置関係を調査したい。

```sql
select
   *
   , row_number() over(partition by SHOPOWNER_ID order by X_METERS) as DIS_NUM
from (
   select distinct
       r.SHOPOWNER_ID = i.SHOPOWNER_ID as DISTINCTFLG
       # 対象会員の予約店舗
       , r.SHOPOWNER_ID
       , r.CLINIC_NAME
       , r.SHOP_LATITUDE
       , r.SHOP_LONGITUDE

       # レコメンド用店舗
       , i.SHOPOWNER_ID as MED_SHOPOWNER_ID
       , i.CLINIC_NAME as MED_CLINIC_NAME
       , i.SHOP_LATITUDE as MED_SHOP_LATITUDE
       , i.SHOP_LONGITUDE as MED_SHOP_LONGITUDE
       , i.CATALOG_ID as MED_CATALOG_ID

       , st_distance(
               st_geogpoint(r.SHOP_LONGITUDE, r.SHOP_LATITUDE)
             , st_geogpoint(i.SHOP_LONGITUDE, i.SHOP_LATITUDE)
         ) as X_METERS
   from
       med_rsv_member r
   left join
       med_info_prep i using(INTEGRATION_ID)
   )
where
       X_METERS <= 1000
   and DISTINCTFLG is false
```


## || cf.
+ [BigQuery GIS による天文データのクエリ](https://cloud.google.com/blog/ja/products/gcp/querying-the-stars-with-bigquery-gis) - GoogleCloud
+ [地理関数](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions?hl=ja) - GoogleCloud
+ [BigQueryで測定系の地理関数を確認してみた(BigQuery GIS)](https://dev.classmethod.jp/articles/bigquery-geography-functions-try/) - DevelopersIO
+ [メモ：BigQueryで２点間の距離を計算する](https://qiita.com/shouta-dev/items/f7797665e325da35daf1) - Qiita
+ [SQLで2点間の緯度経度から距離を測定する方法](http://ueblog.natural-wave.com/2010/09/14/latitude-longitude-sql/) - ueblog
+ [SQLで緯度・経度から２点間の距離を算出する](https://oc-technote.com/mysql/sql%E3%81%A7%E7%B7%AF%E5%BA%A6%E3%83%BB%E7%B5%8C%E5%BA%A6%E3%81%8B%E3%82%892%E7%82%B9%E9%96%93%E3%81%AE%E8%B7%9D%E9%9B%A2%E3%82%92%E7%AE%97%E5%87%BA%E3%81%99%E3%82%8B/) - OC TechNote



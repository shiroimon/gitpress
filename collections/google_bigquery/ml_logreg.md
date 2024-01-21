---
date    : 2022-01-01
title   : 🔍 BQML ロジステック回帰
excerpt : ---
tags    : ["Google BigQuery", "BigQuery ML"]
---
## || ロジステック回帰

GoogleCLoud内部に、*BigQueryML*のチュートリアルが用意されている。（なんて親切！）

クイックスタート＞[BigQuery ML で機械学習モデルを作成する](https://cloud.google.com/bigquery-ml/docs/create-machine-learning-model?hl=ja)

```sql
#standardSQL
-- ロジスティック回帰（分類モデル）
create model `bqml_tutorial.sample_model`
options(model_type='logistic_reg') as
    select
          if(totals.transactions is null, 0, 1) as LABEL
        , ifnull(device.operatingSystem, "") as OS
        , device.isMobile as IS_MOBILE
        , ifnull(geoNetwork.country, "") as COUNTRY
        , ifnull(totals.pageviews, 0) as PV
    from
        `bigquery-public-data.google_analytics_sample.ga_sessions_*`
    where
        _TABLE_SUFFIX between '20160801' and '20170630'
```

```sql
#standardSQL
-- k-means
create or replace model 
bqml_tutorial.london_station_clusters 
options(model_type='kmeans', num_clusters=4) as
with 
    hs as (
        select
            h.start_station_name as station_name
            , if(extract(DAYOFWEEK from h.start_date) = 1 or extract(DAYOFWEEK from h.start_date) = 7
                  , "weekend"
                  , "weekday"
              ) as is_weekday
            , h.duration
            , ST_DISTANCE(ST_GEOGPOINT(s.longitude, s.latitude), ST_GEOGPOINT(-0.1, 51.5))/1000 AS distance_from_city_center
        from 
            `bigquery-public-data.london_bicycles.cycle_hire` h
        join
            `bigquery-public-data.london_bicycles.cycle_stations` s
        on 
            h.start_station_id = s.id
        where
            h.start_date between cast('2015-01-01 00:00:00' as timestamp) and cast('2016-01-01 00:00:00' as timestamp) 
    )
    , stationstats as (
        select
            station_name
            , isweekday
            , avg(duration) as duration
            , count(duration) as num_trips
            , max(distance_from_city_center) as distance_from_city_center
        from 
            hs
        group by 
            station_name, isweekday
    )
select * except(station_name, isweekday) from stationstats;
```
cf.[ロンドンのレンタル自転車のデータセットをクラスタ化するための K 平均法モデルの作成](https://cloud.google.com/bigquery-ml/docs/kmeans-tutorial?hl=ja) - GoogleCloud

## || REFERENCE
+ [分類モデルの作成](https://cloud.google.com/bigquery-ml/docs/logistic-regression-prediction?hl=ja) - GoogleCloud
+ [BigQuery ML でロジスティック回帰してみる](https://qiita.com/_kobacky/items/c4b168001d2eedfbab7f) - Qiita
+ [「BigQuery ML」：SQLで機械学習ってどういうこと？試しにSQLでロジスティック回帰を書いてみた。](https://www.wantedly.com/companies/wantedly/post_articles/129482) - Wantedly,inc.

## || ロジステック回帰

GoogleCLoud内部に、*BigQueryML*のチュートリアルが用意されている。（なんて親切！）

クイックスタート＞[BigQuery ML で機械学習モデルを作成する](https://cloud.google.com/bigquery-ml/docs/create-machine-learning-model?hl=ja)

```sql
#standardSQL
create model `bqml_tutorial.sample_model`
options(model_type='logistic_reg') as (
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
);
```


## || REFERENCE
+ [分類モデルの作成](https://cloud.google.com/bigquery-ml/docs/logistic-regression-prediction?hl=ja) - GoogleCloud
+ [BigQuery ML でロジスティック回帰してみる](https://qiita.com/_kobacky/items/c4b168001d2eedfbab7f) - Qiita
+ [「BigQuery ML」：SQLで機械学習ってどういうこと？試しにSQLでロジスティック回帰を書いてみた。](https://www.wantedly.com/companies/wantedly/post_articles/129482) - Wantedly,inc.

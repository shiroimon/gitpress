---
date    : 2023-03-24
title   : 🔍 エクスポート機能
excerpt : ---
tags    : ["Google BigQuery", "export"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || MOTIVATION

```SQL
export data options (
    uri='gs://エクスポート先のバケット名/csv/sql_*.csv.gz'
    , format='CSV'
    , overwrite=true
    , header=true
    , field_delimiter=',' -- default,
    , compression='GZIP'
) as with 
    target as (
        select * 
        from sample_dataset.sample_table
        order by id
    ) 
select * from target;

```

## || REFERENCE
- [BigQueryのエクスポート機能で、Cloud Storageのバケットにテーブルのデータをエクスポートしてみた](https://dev.classmethod.jp/articles/export_data_to_cloudstorage_bucket_bigquery_export/) -DevelopersIO
- 

---
date    : 2022-01-01
title   : 🔍 ERROR
excerpt : 
tags    : ["Google BigQuery", ""]
---

## || BigQueryのエラー
### |  
```
Query exceeded resource limits. This query used 96791 CPU seconds but would charge only 170M Analysis bytes. 
This exceeds the ratio supported by the on-demand pricing model. 
Please consider moving this workload to the flat-rate reservation pricing model, which does not have this limit. 
96791 CPU seconds were used, and this query must use less than 43500 CPU seconds.
```

### | 
```
To copy a table, the destination and source datasets must be in the same region. 
Copy an entire dataset to move data between regions.
```
作成したクエリの抽出結果（300万レコード程）を一時的にテーブル化しようとしたら、以下のようなエラーが出て困った。。

似たような事象の人が[相談](https://www.googlecloudcommunity.com/gc/Data-Analytics/Location-error-on-BigQuery/m-p/424261)していた。（*2）


### | 
```
```

### | 
```
```



## || REFERENCE
- [データセットのコピーの設定](https://cloud.google.com/bigquery/docs/copying-datasets?hl=ja#setting_up_a_dataset_copy) - GoogleCloud 
- [Location error on BigQuery](https://www.googlecloudcommunity.com/gc/Data-Analytics/Location-error-on-BigQuery/m-p/424261) - GoogleCloud(*2)
- []() -

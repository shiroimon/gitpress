---
date    : 2022-01-01
title   : 🔍 ERROR
excerpt : ---
tags    : ["🔍", "Google BigQuery", "error"]
---

![image](https://github.com/polar-beer/gitpress/assets/28585421/5f209bcf-ceb5-45da-9502-0535582d4b0b)
cf. https://dic.pixiv.net/a/%E3%83%AA%E3%82%BB%E3%83%83%E3%83%88%E3%81%95%E3%82%93

## || BigQueryのエラー集
実務で幾度となく現れるアイツ（≒リセットさん:エラー文）。

次回怒られないように、躓いた内容を適宜メモしていく...同じ轍を踏まないように！


### | ⛑️ 「予算足らんがな！でかい作業したきゃ金積みませ！若くは作業量減らせーｺﾗｰ」
```
Query exceeded resource limits. This query used 96791 CPU seconds but would charge only 170M Analysis bytes. 
This exceeds the ratio supported by the on-demand pricing model. 
Please consider moving this workload to the flat-rate reservation pricing model, which does not have this limit. 
96791 CPU seconds were used, and this query must use less than 43500 CPU seconds.
```
サブクエリをかなりの数ネストした上に、デカいテーブルを使いすぎた。。

（解決策）　https://gitpress.io/c/bigquery/bq_table


### | ⛑️ 「別プロジェクトのデータ使うなら同じリージョンでせい！ｺﾗｰ」
```
To copy a table, the destination and source datasets must be in the same region. 
Copy an entire dataset to move data between regions.
```
作成したクエリの抽出結果（300万レコード程）を一時的にテーブル化しようとしたら、上記のようなエラーが出て困った。。

似たような事象の人が[相談](https://www.googlecloudcommunity.com/gc/Data-Analytics/Location-error-on-BigQuery/m-p/424261)していた。（*2）


### | ⛑️ 「」
```
```



## || REFERENCE
- [データセットのコピーの設定](https://cloud.google.com/bigquery/docs/copying-datasets?hl=ja#setting_up_a_dataset_copy) - GoogleCloud 
- [Location error on BigQuery](https://www.googlecloudcommunity.com/gc/Data-Analytics/Location-error-on-BigQuery/m-p/424261) - GoogleCloud(*2)
- []() -

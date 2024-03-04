---
date   : 2022-04-11
title  : 🔍 SAFE_CAST関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "safe_cast"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || safe_cast()

型変換なのだが、通常の'cast()'変換とは異なる。

データ内に、string や Null を許容して一括変換してくれる。


## || Cf.
+ [BigQueryでNULLや空白があるカラムをintに変換する方法](https://itips.krsw.biz/bigquery-how-to-cast-blank-null-column-to-int/) - ITips
+ [変換規則](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators?hl=ja#conversion_rules) - Google Cloud
+ [変換関数](https://cloud.google.com/bigquery/docs/reference/standard-sql/conversion_functions?hl=ja) - Google Cloud
+ [Google BigQueryの新機能 Standard SQLまとめ](https://techblog.zozo.com/entry/bigquery-standard-sql) - ZOZO TECH BLOG
+ []() - 

---
date    : 2023-01-01
title   : 🔍 TABLESAMPLE SYSTEM句
excerpt : ランダムサンプリング抽出
tags    : ["🔍", "BigQuery", "GoogleCloud"]
---

## || TABLESAMPLE SYSTEM句
### |　こう書く
```SQL
--(使い方)FROM句の末尾にTABLESAMPLE SYSTEM句を追記
select *
from `google_analytics_sample.ga_sessions_*`
TABLESAMPLE SYSTEM(10 PERCENT)
```

### | 嬉しいこと
- テーブルの一部分をランダム抽出できる（BigQueryで使用できるSQLクエリの一部）
- スキャンサイズを減らす＝課金を減らす



## || REFERENCE
- [テーブルのサンプリング](https://cloud.google.com/bigquery/docs/table-sampling?hl=ja) - GoogleCloud
- [【BigQuery】TABLESAMPLE SYSTEMを日本一詳しく解説する](https://ex-ture.com/blog/2023/07/06/%E3%80%90bigquery%E3%80%91tablesample-system%E3%82%92%E8%A7%A3%E8%AA%AC%E3%81%99%E3%82%8B/) -　エクスチュア株式会社ブログ

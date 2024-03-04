---
date   : 2022-03-01
title  : 🔍 INTERSECT関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "intersect", "集合演算子"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || intersect

積集合を取得できる。

### | 積集合

```SQL
#standardSQL
(select * from `TABLE1`)
intersect distinct 
(select * from `TABLE2`)
;
```
cf. [分析入門 - SECTION10](https://gitpress.io/c/bigquery/google_bigquery_10#-intersect---積集合) - GitPress



## || REFERENCE
+ [INTERSECT](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax?hl=ja#intersect) - GoogleCloud
+ [【BigQuery】集合演算子(UNION…)まとめ](https://qiita.com/tatsuhiko_kawabe/items/2537c562c6d99f83e37b) - Qiita

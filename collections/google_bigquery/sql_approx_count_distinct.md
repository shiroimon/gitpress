---
date   : 2022-03-01
title  : approx_count_distinct()
excerpt: BigQueryの近侍集計関数について
tags   : ["Google BigQuery", "SQL", "分析", "approx_count_distinct"]
---

## || 近似集計関数

`approx_count_distinct()`

EDA時や、ざっくり特徴量のカウント等に厳密な結果を求めていない時　`count(distinct HOGE)`　を用いないで、
近侍集計関数を用いると幸せ。（メモリの節約にもなるし）


## || Cf.
+ [Approximate aggregate functions](https://cloud.google.com/bigquery/docs/reference/standard-sql/approximate_aggregate_functions#approximate_aggregate_functions) - Google Cloud
+ [SQLは書けるけどBigQueryは初見の人に贈るざっくりBigQuery](https://zenn.dev/masumomo/articles/e45d1f57cc8025) - Zenn
+ [APPROX_COUNT_DISTINCT](https://docs.oracle.com/cd/E57425_01/121/SQLRF/functions013.htm) - Oracle® Database SQL言語リファレンス

---
date   : 2022-03-02
title  : 🔍 LEAD関数 / LAG関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "分析関数", "lead", "lag"]
---

## || lead()
```sql
LEAD ( scalar_expression [ ,offset(1) ] , [ default(null) ] )   
    OVER ( [ partition_by_clause ] order_by_clause )
```
現在行の後ろの行を持ってくる。

現在行の前の行を持ってくる。のは `lag()`。


## || 用途
+ 前日比や、前週比
+ コホート分析


## || Cf.
+ [【BigQuery】LAG関数，LEAD関数の使い方](https://qiita.com/kota_fujimura/items/cff732bb9acb47510a03) - Qiita
+ [LEAD (Transact-SQL)](https://docs.microsoft.com/ja-jp/sql/t-sql/functions/lead-transact-sql?view=sql-server-ver15) - Microsoft
+ [Analytic function concepts](https://cloud.google.com/bigquery/docs/reference/standard-sql/analytic-function-concepts?hl=ja#navigation-functions) - Google Cloud
+ [Navigation functions - LEAD](https://cloud.google.com/bigquery/docs/reference/standard-sql/navigation_functions?hl=ja#lead) - Google Cloud


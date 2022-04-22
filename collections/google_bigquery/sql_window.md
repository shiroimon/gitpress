---
date   : 2022-03-02
title  : WINDOW句
excerpt: 分析関数
tags   : ["Google BigQuery", "SQL", "window句"]
---

## || WINDOW句

# | 実行順序
WINDOW句を使用したクエリの評価は、通常、次の順序で完了します。

1. FROM
2. WHERE
3. GROUP BY と集計
4. HAVING
5. **WINDOW** ← ココ!
6. QUALIFY
7. DISTINCT
8. ORDER BY
9. LIMIT



```sql
/* 同様の処理は纏められる */
select
    member
    , sum(amount) over w as total_amount
    , count(amount) over w as number_of_amount
from 
    raw_table
window 
    w as (partition by member order by date) ← 纏められる 
```

```sql
/* 組み換え可能 */
select
    member
    , sum(amount) over a as total_amount
    , count(amount) over b as number_of_amount
from 
    raw_table
window 
      a as (partition by member order by date)
    , b as (partition by member order by date)
```

## || cf.
+ [WINDOW句](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax?hl=ja#window_clause) -GoogleCloud

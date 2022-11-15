---
date    : 2022-11-15
title   : PIVOT / UNPIVOT句
excerpt : 
tags    : ["Google BigQuery", "pivot", "unpivot", "横持ち変換"]
---
## || PIVOT句

```sql
from 
    TABLE
pivot(
    [集計関数]
    for [カラム名]
    in ([値：in句の利用])
) as [エイリアス]
```



## || UNPIVOT句



## || REFERENCE
- [SQL ServerのPIVOT句・UNPIVOT句](https://www.casleyconsulting.co.jp/blog/engineer/162/) - CasleyConsulting
- [PIVOT演算子](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax?hl=ja#pivot_operator) - GoogleCloud 

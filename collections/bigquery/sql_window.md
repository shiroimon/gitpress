---
date   : 2022-03-02
title  : 🔍 WINDOW句
excerpt: ---
tags   : ["Google BigQuery", "SQL", "window句"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || WINDOW句
> WINDOW句は名前付きウィンドウのリストを定義します。
> 
> 名前付きウィンドウは、分析関数を使用するテーブル内の行のグループを表します。
> 名前付きウィンドウは、ウィンドウ指定で定義するか、別の名前付きウィンドウを参照することが可能です。
> 
> 別の名前付きウィンドウを参照している場合、参照されるウィンドウは、参照するウィンドウよりも前に定義されている必要があります。

### | 実行順序
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
    w as (partition by member order by date)
group by 
    member
;
```

```sql
/* 組み換え可能 */
select
    member
    , sum(amount) over a as total_amount
    , count(amount) over b as number_of_amount
    , count(amount) over(c rows between PRECEDING and FOLLOWING) as number_of_amount
from 
    raw_table
window 
      a as (partition by member order by date)
    , b as (partition by member order by date desc, class_a)
    , c as b --こんなのもできる
group by 
    member
;
```

cf.
- [WINDOW句](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax?hl=ja#window_clause) -GoogleCloud

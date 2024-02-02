---
date   : 2022-04-22
title  : 🔍 QUALIFY句
excerpt: 分析関数の結果をフィルタリング
tags   : ["Google BigQuery", "QUALIFY"]
---

## || QUALIFY 句

    QUALIFY bool_expression

> QUALIFY 句は、分析関数の結果をフィルタリングします。<br>
> 分析関数は QUALIFY 句または SELECT リストに存在する必要があります。<br>
> <br>
> bool_expression が TRUE と評価された行のみが含まれます。<br>
> bool_expression が NULL または FALSE と評価された行は破棄されます。


### | 実行順序

QUALIFY 句を使用したクエリの評価は、通常、次の順序で完了します。

1. FROM
2. WHERE
3. GROUP BY と集計
4. HAVING
5. WINDOW
6. **QUALIFY** ← ココ!
7. DISTINCT
8. ORDER BY
9. LIMIT



```sql
with
    score_with_rank as (
        select
            student_id
            , class
            , score
            , rank() over(
                  partition by class
                  order by score desc
              ) as rank
        from
           scores
    )
select 
    *
from
    score_with_rank
where
    rank >= 3
;
```

（↑コレが、こう↓）

```sql
/* with句による多重ネスト回避時に使える */
select
    student_id
    , class
    , score
    , rank() over (
          partition by class
          order by score desc
      ) as rank
from
    scores
where true
    qualify rank >= 3
;
```

```
i.e.
    qualify句を使用する場合、以下のいずれかがクエリ内で使用されている必要がある。
    よって、いずれにも該当しない場合は、面倒だが今回のように where true のような指定をすることになる。
    　・where
    　・group by
    　・having
```

```sql
/* 絞込要因で、SELECT句持ちしたくない時に使える */
select
    student_id
    , class
    , score
from
    scores
qualify 
    rank() over (partition by class order by score desc) = 1 -- rank 1
;
```

## || REFERENCE
+ [QUALIFY 句](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax?hl=ja#qualify_clause) - GoogleCloud
+ [BigQueryのqualify句の使い方](https://zenn.dev/hrkh/articles/hatena-20210615-124624) - Zenn
+ [tips9: preview 機能を楽しむ](https://yoheikikuta.github.io/BigQuery_tips_part3/) - 原理的には可能
+ 

## || QUALIFY 句

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
```sql
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


## || ｃf.
+ [BigQueryのqualify句の使い方](https://zenn.dev/hrkh/articles/hatena-20210615-124624) - Zenn

---
date   : 2022-03-01
title  : EXCEPT演算子
excerpt:
tags   : ["Google BigQuery", "SQL", "except"]
---

## || except

差集合を取得できる。

### | 差集合


cf. []() - 


### | 除外利用
```sql
select
    cd.* except(VAR_KEY, VAR_VALUE)
    , h.NUMBERS as PARK_MEMBER_ID
from 
    `datasets.table` cd
left join 
    hoge_id h using(VAR_VALUE)
```
「cd」「ｈ」何れにも同カラム名が存在するときに、selectで指定する際に除外するときにも使える。



## || cf.
+ [SQL ServerのEXCEPT　差（差集合）を取得する](https://sql-oracle.com/sqlserver/?p=467) - SQLServer初心者でもスッキリわかる
+ [セット演算子 - EXCEPT および INTERSECT (Transact-SQL)](https://docs.microsoft.com/ja-jp/sql/t-sql/language-elements/set-operators-except-and-intersect-transact-sql?view=sql-server-ver15) - Microsoft Build

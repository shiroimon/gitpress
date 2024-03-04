---
date   : 2022-03-01
title  : 🔍 EXCEPT演算子
excerpt: ---
tags   : ["Google BigQuery", "SQL", "except"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || except

差集合を取得できる。

### | 差集合

```SQL
#standardSQL
(select * from `TABLE1`)
except distinct 
(select * from `TABLE2`)
;
```
cf. [分析入門 - SECTION10](https://gitpress.io/c/bigquery/google_bigquery_10#-except---差集合) - GitPress


### | 除外利用
```sql
#standardSQL
select
      cd.* except(VAR_KEY, VAR_VALUE)
    , h.NUMBERS as PARK_MEMBER_ID
from 
    `datasets.table` cd
left join 
    hoge_id h using(VAR_VALUE)
;
```

    「cd」内に存在するカラム「VAR_KEY, VAR_VALUE」を、selectで指定する際に除外するときにも使える。



## || REFERENCE
+ [SQL ServerのEXCEPT　差（差集合）を取得する](https://sql-oracle.com/sqlserver/?p=467) - SQLServer初心者でもスッキリわかる
+ [セット演算子 - EXCEPT および INTERSECT (Transact-SQL)](https://docs.microsoft.com/ja-jp/sql/t-sql/language-elements/set-operators-except-and-intersect-transact-sql?view=sql-server-ver15) - Microsoft Build

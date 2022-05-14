---
date   : 2022-05-11
title  : EXTRUCT関数
excerpt:
tags   : ["Google BigQuery", "SQL", "extruct"]
---

## || EXTRACT

```sql
select 
    extract(year from rsv_date from) as YEAR
    , extract(month fron rsv_date) as MONTH
```


## || cf.
+ [【SQL日付関数】EXTRAC – 日付から任意の日付要素を取得する　（Oracle）](http://www.sql-master.net/articles/SQL761.html) - SQL Master　データベースエンジニアとセキュリティエンジニアとLinuxエンジニアのための情報
+ 

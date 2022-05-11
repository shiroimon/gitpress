---
date   : 2022-05-11
title  : extruct()
excerpt:
tags   : ["Google BigQuery", "SQL", "extruct"]
---

## || EXTRACT
```sql
extract(element FROM date)
```

```sql
select 
    extract(rsv_date from year) as YEAR
    , extract(rsv_date from month) as MONTH
```


## || cf.
+ [【SQL日付関数】EXTRAC – 日付から任意の日付要素を取得する　（Oracle）](http://www.sql-master.net/articles/SQL761.html) - SQL Master　データベースエンジニアとセキュリティエンジニアとLinuxエンジニアのための情報
+ 

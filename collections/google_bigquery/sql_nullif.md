---
date   : 2022-05-11
title  : NULLIF関数
excerpt: 
tags   : ["Google BigQuery", "SQL", "nullif()"]
---

## || nullif()

nullifは、故意にnull値を用いることができる。

0除算時のエラー回避して、nullを返す。


### | e.g.
nullif()の中身で行われていることは、以下のcase式と同様。
```sql
nullif(TOTALAMOUNT, 0) 
```
```sql
case 
    when TOTALAMOUNT=0 then null
    else TOTALAMOUNT
end 
```


## || cf.
+ [知っておくと便利な関数 - NULLIF](https://sql55.com/t-sql/t-sql-nullif.php) - SQLServer入門
+ [SQL: NULLIF() 関数を使ってゼロ除算を防ぐ](https://blog.amedama.jp/entry/2017/06/29/204834) - CUBE SUGAR CONTAINER
+ [0除算エラーを回避する (SQLの構文)](https://www.ipentec.com/document/sql-error-0-division) - iPentec
+ []() - 

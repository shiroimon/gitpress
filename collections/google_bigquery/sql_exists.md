---
date   : 2022-03-01
title  : exists
excerpt: BigQueryのexistsについて
tags   : ["Google BigQuery", "SQL", "exists"]
---

## || exists

```sql
select * from hoge
where exists(
    [※サブクエリ]
    );
```
テーブル結合を伴わなくとも親クエリ内にあるカラム指定をしてソートすることができる。

※ `in句`とは似て非なる。


## || Cf.
+ [【SQL】5分でわかる！ EXISTSでサブクエリを扱う方法](https://www.sejuku.net/blog/73615) - SAMURAIENGIEER
+ []() - 
+ []() - 


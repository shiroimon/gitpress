---
date   : 2022-03-01
title  : 🔍 EXISTS句（式）
excerpt: ---
tags   : ["Google BigQuery", "SQL", "exists"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || exists()

> exists : 意味・対訳
>
> 存在する、現存する、(特殊な条件または場所に)ある、現われる、生存する、生きている、(…で)(やっと)生きていく
> 
> cf. https://ejje.weblio.jp/content/exist

```sql
select * from hoge
where exists(
    [※サブクエリ]
    );
```
テーブル結合を伴わなくとも親クエリ内にあるカラム指定をしてソートすることができる。

※ `in句`とは似て非なる。



## || cf.
+ [【SQL】5分でわかる！ EXISTSでサブクエリを扱う方法](https://www.sejuku.net/blog/73615) - SAMURAIENGIEER
+ []() - 
+ []() - 


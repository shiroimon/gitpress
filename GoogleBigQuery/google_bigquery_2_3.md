---
date   : 2021-09-11
title  : 【BigQuery】分析入門 - Session2~3
excerpt: Google BigQuery基本の「き」について。
tags   : ["Google BigQuery", "SQL基本", "分析基本"]
---

## || Google BigQueryとは？
![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

ざっくり、Googleが提供するGoogle Cloud Platform上で動作するSQLを書くことができる機能です。


## || BigQuery環境構築
![project](https://i.gyazo.com/edac850c69d81a2eccfa28c349bd5e09.png)


![finaly](https://i.gyazo.com/45e5e6f63178156c3edc62d5f53c44a2.png)
Cf.完成図


1. プロジェクトを作成
![make project](https://i.gyazo.com/98551e2f0aea93e9506d6d018c2ace9f.png)
```
```

2. データセットを作成
![make_ds](https://i.gyazo.com/592db3e492533ab6c672ee8ac172720a.png)
```
```

3. テーブルを作成
![make_table](https://i.gyazo.com/a321a6ed34a70f197338198ec69ddcd5.png)
```
```

## || SQL
### ● 基本文法
```SQL
SELECT {カラム名}
FROM {テーブル名};
```
{テーブル名}から、{カラム名}に記載の列を取得してきて！って意味です。


### ● 記述、実行順序
```SQL
/********************************************************************
 * ■ 各区の記述順序
 ********************************************************************/
# 1. SELECT    ：取得する列の指定
#    （集計関数）
# 2. FROM      ：取得するテーブルの指定
# 3. JOIN      ：テーブルの結合の処理
#    ON / USING：結合に利用するキー指定
# 4. WHERE     ：絞り込み条件の指定
# 5. GROUP BY  ：グループ化項目の指定
# 6. HAVING    ：グループ化された集計結果に対して絞り込み条件の指定
# 7. ORDER BY  ：並び替え条件
# 8. LIMIT     ：表示する行数の指定
```

```SQL
/********************************************************************
 * ■ 各区の実行順序
 ********************************************************************/
#  1. FROM
#  2. ON / USING
#  3. JOIN
#  4. WHERE
#  5. GROUP BY
#  6.   (集計関数)
#  7. HAVING
#  8. SELECT
#  9. ORDER BY
# 10. LIMIT
```


### ● 基本関数一覧

- [x] `SUM()`
- [x] `AVG()`
- [x] `COUNT()`
- [x] `MIN()`
- [x] `MAX()`
- [x] `STDDEV_POP()`
- [ ] `STDDEV_SAMP()`
- [x] `ROUND()`
- [ ] `CEIL()`
- [ ] `FLOOR()`
- [ ] `TRUNC()`
- [ ] `ABS()`
- [ ] `MOD()`
- [x] `CAST()`
- [x] `CONCAT()`


```SQL
# ----------------------------
# |16| LENGTH()          |||
# |17| SUBSTR()          |||
# |18| REPLACE()         |||
# |19| TRIM()            |||
# |20| REGEXP_CONTAINS() |||
# |21| REGEXP_EXTRACT()  |||
# |22| REGEXP_REPLACE()  |||
# |22| DATE()            |||
# |23| DATETIME()        |||
# |24| CURRENT_DATE()    |||
# |25| CURRENT_DATETIME()|||
# |26| DATE_ADD()        |||
# |27| DATETIME_ADD()    |||
# |28| DATE_SUB()        |||
# |29| DATETIME_SUB()    |||
# |30| DATE_TRUNC()      |||
# |31| DATETIME_TRUNC()  |||
# |32| FORMAT_DATE()     |||
# |33| FORMAT_DATETIME() |||
# ----------------------------
```


### ● 便利な句
```SQL
IN()
```

```SQL
# | 1| IF()        |||
# | 2| IFNULL()    |||
```
```SQL
# | 3| CASE WHEN THEN ELSE END |||
# | 4| CASE()          |||
# |  |      WHEN THEN  |||
# |  |      WHEN THEN  |||
# |  |      ELSE       |||
# |  | END             |||
```
```SQL
# | 5| []() OVER(                |||
# |  |           PARTITION BY    |||
# |  |           ORDER BY        |||
# |  |           (WINDOW_FRAME)  |||
# |  |          )                |||
```

```SQL
# | 6| WITH                      |||
# |  |  [] AS (SELECT FROM ),    |||
# |  |  [] AS (SELECT FROM )     |||
```


### ● 結合
```SQL
# ■ 結合句
# INNER JOIN
# LEFT OUTER JOIN
# RIGHT OUTER JOIN
# FULL OUTER JOIN
```
```SQL
FROM [table①] AS [t①]
[結合句] [table②] AS [t②]
→ USING [key]
→ ON [t①.key = t②.key]
```

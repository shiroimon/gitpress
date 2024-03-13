---
date    : 2024-01-01
title   : 🔍 CTE
excerpt : ---
tags    : ["🔍", "BigQuery", "GoogleCloud", "cte", "analysis"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || CTE
> CTEとは `Common Table Expressions` の略で、OracleやPostgreSQLにはすでにあった機能であるため知っている方もいるかもしれません。
> CTEは単一ステートメントのスコープ内に存在し、あとでそのステートメント内で複数回参照できる名前付き一時結果セットです。
> SQL内で有効な名前付きテンポラリーテーブルを作成する、といったイメージだと理解しやすいかと思います。
> MySQLではバージョン8.0から採用されており、さらに8.0.19では再帰CTEのSELECT部分でLIMIT句を使用することが可能になりました。
>
> 共通テーブル式（CTE）を利用するにはWITH句を使用します。
>
> cf. [SQLの共通テーブル式（CTE）を使ってみよう](https://gihyo.jp/article/2022/10/mysql-rcn0181) -gihyo.jp

要はコレhttps://gist.github.com/sh16ma/87881882ee65dd1829e85cbe484482f9



## || REFERENCE
- [SQLの共通テーブル式（CTE）を使ってみよう](https://gihyo.jp/article/2022/10/mysql-rcn0181) -gihyo.jp
- [13.2.15 WITH (共通テーブル式)](https://dev.mysql.com/doc/refman/8.0/ja/with.html) -MySQL
- [What Is a Common Table Expression (CTE) in SQL?](https://learnsql.com/blog/what-is-common-table-expression/) -LearnSQL
- [What Is a CTE?](https://learnsql.com/blog/what-is-cte/) -LearnSQL
- [CTE in SQL](https://www.geeksforgeeks.org/cte-in-sql/) -geeksforgeeksi



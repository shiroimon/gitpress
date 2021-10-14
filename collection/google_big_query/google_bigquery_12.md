---
date   : 2021-09-02
title  : 【BigQuery】分析入門 - Section12
excerpt: Google BigQuery基本の「き」について。
tags   : ["Google BigQuery", "SQL基本", "分析基本"]
---

### ● Section12 : おまけ（可視化）
![Google DataPortal](https://i.gyazo.com/625fcf38f301105c0dbc76da5b9951ff.png)

これまでのsectionで学んできたデータを、

テーブルデータとしてではなく視覚的に伝えやすくする方法が可視化です。

1. SQLをBigQueryに書く。
```SQL
SELECT
        sp.year_month,
        sm.shop_name,
        sp.ttl_sales
FROM(
        SELECT
                DATE_TRUNC(date, MONTH) AS year_month,
                shop_id,
                SUM(sales_amount) AS ttl_sales
        FROM `prj-test3.bq_sample.shop_purchases`
        GROUP BY date, shop_id) AS sp
LEFT JOIN `prj-test3.bq_sample.shops_master` AS sm
USING(shop_id);
```

2. BigQuery上で、求めたいデータを抽出する。
![biqquery_editer](https://i.gyazo.com/d672afedd1bcc9c0678bbba6ff5bb9dd.png)
```
表示画面の上部のナビゲーションから、「データを探索」をプルダウン。
そのまま、データポータルへ遷移。
```

3. Google DataPortal
![Google DataPortal](https://i.gyazo.com/625fcf38f301105c0dbc76da5b9951ff.png)
```
よかったら以下にリンクを貼っておきますので、覗いてみてください。
こんな感じにお洒落になりました。
```
[🔗 作成したデータポータルはこちら >](https://datastudio.google.com/reporting/fa043ba1-256c-49a9-8f1b-bf12bf5295fa)

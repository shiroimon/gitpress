---
date   : 2021-09-11
title  : 🔍 分析入門
excerpt: - Session2~3
tags   : ["Google BigQuery", "SQL", "分析", "Udemy講座"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || Google BigQueryとは？

>BigQuery（ビッグクエリ）は、ペタバイト単位のデータに対するスケーラブルな分析を可能にする、フルマネージドのサーバーレスのデータウェアハウスである。ANSI SQLを使用したクエリをサポートするPlatform as a Service（PaaS）としてGoogle Cloud Platformにより提供されている。また、機械学習の機能も組み込まれている。BigQueryは2010年5月に発表され、2011年11月に一般提供（GA）となった。
>
> Cf. [BigQuery - Wikipedia](https://ja.wikipedia.org/wiki/BigQuery)

>適切なハードウェアとインフラストラクチャを用意せずに大規模なデータセットを保存し、それに対してクエリを実行すると、時間と費用がかかってしまいます。エンタープライズ データ ウェアハウスである Google BigQuery は、Google のインフラストラクチャの処理能力を活用して SQL クエリを超高速で実行し、こうした問題を解決します。データを BigQuery に読み込んだら、後の処理は Google にお任せください。また、データの表示やクエリの実行権限を自分以外のユーザーに付与できるため、業務上の必要に応じてプロジェクトや自分のデータに対するアクセスを制御することも可能です。
>
> Cf. [BigQuery とは - Google Cloud](https://cloud.google.com/bigquery/docs/introduction?hl=ja)

ざっくり、Googleが提供するPaaS（Platform as a Service）のこと。



### | データベース
```
2種類のサービス形態がある。
平たくまとめるとお金を取るか、取らないか。無料で利用できるのがOSS。
BigQueryは無料枠もあるが、処理に応じて従量課金型。
```
 * OSS (Open Source Softwear)

   MySQL、PostgreSQL、SQLitte...etc.

 * SaaS

   Google BigQuery、Amazon AWS、TreasureData...etc.



## || BigQuery環境構築
![project](https://i.gyazo.com/edac850c69d81a2eccfa28c349bd5e09.png)

* まずは、BigQueryにアクセスしてみよう。
https://cloud.google.com/bigquery/

* 完成イメージとしては以下のような図。
![finaly](https://i.gyazo.com/4ad2aaf572a3e100a4dc2aa008df32e4.png)
```
外枠にプロジェクトがあり、その中にデータセットがあり、更にその中に各テーブル（Excelでいうタブ）がある。
入れ子（ネスト）構造。
```

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


## | 基本文法
```SQL
SELECT {カラム名}
FROM {テーブル名};
```
{テーブル名}から、{カラム名}に記載の列を取得してきて！って意味です。


### | 記述、実行順序
* 各区の記述順序
```SQL
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

* 各区の実行順序
```SQL
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

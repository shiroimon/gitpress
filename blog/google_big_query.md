---
title: 【BigQuery】
date: 2021-09-01
tag: ["Google BigQuery", "SQL基本構文", "分析"]
excerpt: Google BigQueryについて。
---
## || はじめに
この記事は、Udemyにて学習をした際のメモです。<br>
コース受講したい場合は、ページ最下部に参考リンクとして掲載いたします。<br>
（アフィリエイトリンク等ではなく、純粋に学習を通して大変参考になったのでm(__)m ）

初めてのBigQueryでしたが、これからSQLでガシガシ分析できると思うと楽しみです。


## || Google BigQueryとは？
![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)



## || BigQuery環境構築
![project](https://i.gyazo.com/edac850c69d81a2eccfa28c349bd5e09.png)


Cf.完成図<br>
![finaly](https://i.gyazo.com/45e5e6f63178156c3edc62d5f53c44a2.png)


### ●プロジェクトを作成
![make project](https://i.gyazo.com/98551e2f0aea93e9506d6d018c2ace9f.png)


### ●データセットを作成
<!-- ![make_ds_tab](https://i.gyazo.com/59f03d471c8e280a290f721e8e642e73.png) -->

![make_ds](https://i.gyazo.com/592db3e492533ab6c672ee8ac172720a.png)

### ●テーブルを作成

<!-- ![make_table_pulldown](https://i.gyazo.com/f4d1bad318675df78d20c7f3ec0a05fd.png) -->

<!-- ![make_table_button](https://i.gyazo.com/3f63ff11956f047796baae9bb4a64cbd.png) -->

![make_table](https://i.gyazo.com/a321a6ed34a70f197338198ec69ddcd5.png)


## || SQL
### ● 基本文法
```SQL
SELECT {カラム名}
FROM {テーブル名};
```
(読み) {テーブル名}から、{カラム名}に記載の列を取得してきて！って意味です。


### ● 記述、実行順序
```SQL
/********************************************************************
 * ■ 各区の記述順序
 ********************************************************************/
# 1. SELECT    ：取得する列の指定
#    （集計関数）：
# 2. FROM      ：取得するテーブルの指定
# 3. WHERE     ：絞り込み条件の指定
# 4. GROUP BY  ：グループ化項目の指定
# 5. HAVING    ：グループ化された集計結果に対して絞り込み条件の指定
# 6. ORDER BY  ：並び替え条件
# 7. LIMIT     ：表示する行数の指定
```
```SQL
/********************************************************************
 * ■ 各区の実行順序
 ********************************************************************/
# 1. FROM
# 2. WHERE
# 3. GROUP BY
# 4. (集計関数)
# 5. HAVING
# 6. SELECT
# 7. ORDER BY
# 8. LIMIT
```

### ● 基本関数一覧
||関数名|読み|用途|
|-|-|-|-|
|1|SAM()|サム|合算値|
|2|AVG()|アベレージ|平均値|
|3|||




## || 学習メモ
この章は、完全個人メモです。 <br>
（後に自分で見返すようになっております。） <br>

どんな考え方（ロジック）でコードを書き上げたのか、どこに躓いたのか、<br>
後に俯瞰したいためにお恥ずかしながら演習問題で間違えてしまったコードはそのまま記載してます。悪しからず...

* [Section4](###_●_Section4): 基本文法
* [Section5](###_●_Section5): 集計関数
* Section6: 四則演算、条件分岐
* Section7: 分析関数
* Section8
* Section9
* Section10
* Section11
* Section12


### ● Section4
```SQL
/**************************************************************
 * 【Uddemy】BigQueryで学ぶ非エンジニアのためのSQLデータ分析入門
 *  section : 4
 **************************************************************/

# ■ SELECT句 - かラム指定
# 【演習問題1】
-- SELECT purchase_id, user_id FROM bq_sample.shop_purchases;

# ■ DISTINCT句 - 重複除外
# 【演習問題2】
-- SELECT DISTINCT user_id FROM bq_sample.shop_purchases;
-- SELECT DISTINCT user_id, shop_id FROM bq_sample.shop_purchases;

# ■ EXPECT() - かラム除外
# 【演習問題3】
-- SELECT * FROM bq_sample.shop_purchases; #(70.1KB)
-- SELECT * EXCEPT(shop_id, product_id) FROM bq_sample.shop_purchases; #(50.1KB)

/********************************************************************/
# ■ ORDERBY句 - ソート機能(ASC:昇順 DESC:降順）

# 【4.4 演習問題1(:)】
-- SELECT * FROM bq_sample.shop_purchases ORDER BY user_id;
-- SELECT user_id, quantity FROM bq_sample.shop_purchases ORDER BY quantity desc;

# 【4.4 演習問題2(5:06)】
-- SELECT *
-- FROM bq_sample.shop_purchases
-- ORDER BY date, sales_amount desc, quantity desc; #日付順(昇順) ＋ 同日の場合売上高降順 + 数量降順

# 【4.4 演習問題3(8:20)】
# eg. ORDER BY {カラム番号}指定可能。… [1]indexに注意
-- SELECT *
-- FROM bq_sample.shop_purchases
-- ORDER BY 3 desc, 7;
-- -- ORDER BY data desc, sales_amount;


/********************************************************************/
# ■ LIMIT句 - レコード制限
# ※表示レコード数を制限するだけで、後ろの計算は全行行われている。
#   そのため発生料金に対して節約できるわけではない。

# 【4.5 演習問題1 (0:55)】
-- SELECT *
-- FROM bq_sample.shop_purchases
-- LIMIT 5;

# 【4.5 演習問題2 (2:23)】
-- SELECT *
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY date desc
-- LIMIT 10;

# 【4.5 演習問題3 (:)】取得制限
-- SELECT purchase_id, sales_amount
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY sales_amount desc
-- LIMIT 5 OFFSET 11;


/********************************************************************/
# ■ WHERE句 - 絞り込み条件
# 【4.6/4.7/4.8】
# eg.「SELECT * FROM bq_sample.shop_purchases WHERE quantity > 3;」

/***********
 * 数字型
 ***********/
# eg. WHERE sales-amount > 10000      -- 売上高¥10,000以上のレコード
# eg. WHERE quantity <= 3             -- 数量3以下のレコード

# eg. WHERE list_price BETWEEN 8500 and 9800     -- ¥8,500~¥9,800に該当レコード
# eg. WHERE list_price NOT BETWEEN 8500 and 9800 -- ¥8,500~¥9,800に該当レコード以外

# eg. WHERE quantity IN(3, 7, 11)     -- 数量が「3」「7」「11」にに該当レコード
# eg. WHERE quantity NOT IN(3, 7, 11) -- 数量が「3」「7」「11」にに該当レコード以外

# 【4.7 演習問題1(:)】
-- SELECT user_id, quantity
-- FROM bq_sample.shop_purchases
-- WHERE quantity > 3
-- ORDER BY 1;

# 【4.7 演習問題2(7:15)】
-- SELECT user_id, sales_amount
-- FROM bq_sample.shop_purchases
-- WHERE sales_amount BETWEEN 5000 AND 10000;

# 【4.7 演習問題3(8:15)】
-- SELECT user_id, date, sales_amount, shop_id
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE shop_id IN(1, 3, 4);


/***********
 * 文字型
 ***********/
# eg. WHERE last-name = "鈴木"
# eg. WHERE last-name != "田中"

# eg. WEHER last_name IN("山田", "鈴木", "田中")
# eg. WEHER last_name NOT IN("山田", "鈴木", "田中")

# eg. WHERE first_name LIKE "美%"  --名前が「美○」の後方一致レコード eg.美咲
# eg. WHERE first_name LIKE "%美"  --名前が「○美」の後方一致レコード eg.愛佑美
# eg. WHERE first_name LIKE "%美%" --名前が「○美○」の部分一致レコード eg.美咲,愛佑美
# eg. WHERE first_name LIKE "__子"  -- 名前が「(2文字)子」のレコード eg.由香子

# 【4.8 演習問題1(3:22)】
-- SELECT *
-- FROM bq_sample.customers
-- WHERE last_name = "前田";

# 【4.8 演習問題2(4:15)】
-- SELECT *
-- FROM `prj-test3.bq_sample.customers`
-- WHERE first_name IN ("愛","愛子","愛美");

# 【4.8 演習問題3(5:15)】
-- SELECT *
-- FROM `prj-test3.bq_sample.customers`
-- WHERE first_name NOT LIKE "%子"
-- ORDER BY gender desc;


/***********
 * 日付型
 ***********/
# eg. WHERE date >= "2017-01-01"
# eg. WHERE date != "2017-01-01"
# eg. WHERE date IN("2017-07-01", "2017-07-02")
# eg. WHERE timestamp BETWEEN "2017-07-01 00:09:00" AND "2017-07-01 18:00:00"

# 【4.9 演習問題1(2:00)】
-- SELECT *
-- FROM bq_sample.shop_purchases
-- -- ORDER BY date
-- -- LIMIT 10
-- WHERE date <= "2018-06-01";

# 【4.9 演習問題1(5:00)】
-- SELECT *
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE date NOT BETWEEN "2018-04-29" AND "2018-05-06";


/***********
 * 論理型
 ***********/
# eg. WHERE is_premium IS TRUE
# eg. WHERE is_premium IS FALSE
# eg. WHERE is_premium IS NOT TRUE  -- False
# eg. WHERE is_premium IS NOT FALSE -- True

# 【4.10 演習問題1(1:20)】
-- SELECT user_id, last_name
-- FROM bq_sample.customers
-- WHERE is_premium IS FALSE;


/********************************************************************/
# ■ WHERE句 - 複数条件（論理演算子）
# eg. WHERE gender = 2 AND is_premium IS TRUE
# eg. WHERE gender = 2 AND is_premium IS TRUE AND prefecture = "北海道"
# eg. WHERE (gender = 2 AND is_premium IS TRUE) OR prefecture = "北海道"

# 【4.11 演習問題1(3:20)】
-- SELECT user_id, last_name, first_name
-- FROM `prj-test3.bq_sample.customers`
-- WHERE (is_premium IS FALSE AND gender = 1) OR birthday < "1970-12-31"
-- ORDER BY user_id
-- LIMIT 5;


/********************************************************************/
# ■ WHERE句 - NULLの振る舞い
# eg. WHERE gender = 2 AND birthday IS NOT NULL

# 【4.12 演習問題1(2:00)】
-- SELECT user_id, last_name, first_name
-- FROM `prj-test3.bq_sample.customers`
-- WHERE first_name IS NULL;


/********************************************************************/
# ■ AS句 - かラム名上書き
# eg. SELECT user_id,
#            date AS purchase_data #←dateを何の日かを説明補完できる（日 as 購入日）
#     FROM bq_sample.customers;
# （メモ）AS句は、「AS」と記載しなくとも半角スペースでも同様の挙動となる。
#         可読性の観点から、明記した方が親切。

# 【4.13 演習問題1(1:50)】
-- SELECT user_id, sales_amount AS salse_in_JPY
-- FROM `prj-test3.bq_sample.shop_purchases`;

# 【4.13 演習問題2(3:00)】
-- SELECT user_id, sales_amount AS salse_in_JPY
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY salse_in_JPY desc
-- LIMIT 5;

# （メモ）WHERE句の中ではAS句で指定したカラム名は使用できない。
#         ∵ 実行順序の関係によるため。(Cf. 5.10)
# 【4.13 演習問題2(5:00)】
-- SELECT user_id, sales_amount AS salse_in_JPY
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE salse_in_JPY > 10000 #error. Unrecognized name: salse_in_JPY
-- ORDER BY salse_in_JPY desc
-- LIMIT 5;

```
<!-- ----------- section4 END ----------- -->

### ● Section5
```SQL
/**************************************************************
 * 【Uddemy】BigQueryで学ぶ非エンジニアのためのSQLデータ分析入門
 *  section : 5
 **************************************************************/

# ■ COUNT() - 行数集計
# eg. SELECT COUNT(*) FROM sample.shop_purchases;
# eg. 「個数が１のレコード数をカウントしたい」※行数のカウントであってデータ個数ではない
#      SELECT COUNT(*)
#      FROM sample.shop_purchases
#      WHERE quantity = 1;

#【5.2 演習問題1(2:09)】
-- SELECT COUNT(*) AS gyou_su
-- FROM `prj-test3.bq_sample.customers`
-- WHERE gender = 2 AND birthday >= "1989-01-08";


# ■ COUNT() - データ個数集計
# eg. SELECT COUNT(かラム名)

# ■ カテゴリカル変数の分類集計
# eg. SELECT COUNT(DISTONCT first_name)

#【5.2 演習問題2(6:50)】
-- SELECT COUNT(*) AS gyou_su,
--        COUNT(first_name) AS data_kosu,
--        COUNT(DISTINCT first_name) AS shurui_su
-- FROM `prj-test3.bq_sample.customers`
-- WHERE gender = 2;


/********************************************************************/
# ■ GROUPBY句 - グループ化
# データから意味を読み取るには。
# 「質的データ（定性）」項目でグループを作る。「量的データ（定量）」を集計する。
#
# eg. 店舗ID別のレコード数が知りたい。
#     SELECT shop_id,
#            COUNT(*) AS No_of_purchase
#     FROM `prj-test3.bq_sample.shop_purchases`
#     WHERE date BETWEEN "2018-01-01" AND "2018-06-30"
#     GROUP BY shop_id       ←店舗IDでまとめる(※SELECTに書いてないかラム名だとerror)
#     ORDER BY shop_id
#     LIMIT 5;
#
#     | |shop_id|No_of_purchase|
#     |1|      1|           265|
#     |2|      2|           207|
#     |3|      3|            87|

#【5.4 演習問題1(3:30)】
-- SELECT product_id, COUNT(*) AS No_of_purchase
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY product_id
-- ORDER BY NO_of_purchase DESC;
-- # | |product_id|No_of_purchase|
-- # |1|        11|           102|
-- # |2|        10|            98|
-- # |3|        13|            93|


#【5.4 演習問題2(6:20)】
-- SELECT prefecture,
--        COUNT(*) AS no_of_menbers
-- FROM `prj-test3.bq_sample.customers`
-- GROUP BY 1
-- ORDER BY 2 DESC;
-- # | |prefecture|no_of_menbers|
-- # |1|Tokyo     |          582|
-- # |2|Osaka     |           88|
-- # |3|Kanagawa  |           72|


#【5.4 演習問題3(8:00)】
# (miss_code)
-- SELECT shop_id,
--        COUNT(shop_id) AS No_of_purchases_by_shop,
--        COUNT(product_id) AS No_of_purchases_by_product
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY shop_id
-- ORDER BY 2 >= 3;

#（重要）SELECTの順番が問われる。∴GROPBYの順番を変更しても同様の挙動。
-- SELECT shop_id,
--        product_id,
--        COUNT(*) AS NO_of_purchase
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY shop_id, product_id
-- ORDER BY shop_id, product_id;
-- # | |shop_id|product_id|No_of_purchase|
-- # |1|      1|         1|            17|
-- # |2|      1|         2|            17|
-- # |1|      1|         3|            10|


/********************************************************************/
# ■ SUM() - 合計値
# eg. SELECT SUM(quantity) FROM bq_sample.shop_purchases;
#    | |f0_ |
#    |1|2895|

#【5.5 演習問題1(1:56)】
-- SELECT shop_id, SUM(quantity) AS Total_quantity
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY 1
-- ORDER BY 2 DESC;
-- #| |shop_id|Taotal_quantity|
-- #|1|      1|           1257|
-- #|2|      2|           1063|
-- #|3|      3|            460|


#【5.5 演習問題2(3:30)】
-- SELECT shop_id, SUM(sales_amount) AS Total_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE product_id IN(1, 5, 11, 18)
-- GROUP BY shop_id
-- ORDER BY Total_sales DESC;
-- #| |shop_id|Total_salse|
-- #|1|      1|    1789674|
-- #|2|      2|    1422230|

# ■ ※集計計算時はNULLが無視される
# eg. null_treatment.csvを確認
#     SELECT *
#     FROM `prj-test3.bq_trial.null_treatment` #←nullの多いデータ
#     ORDER BY 1;

# eg. SELECT SUM(quantity) AS ttl_qty, SUM(sales) AS ttl_salse
#     FROM `prj-test3.bq_trial.null_treatment`;
#     #| |ttl_qty|ttl_salse|
#     #|1|      9|    49600|

# eg. SELECT shop_name,
#            SUM(quantity) AS ttl_qty,
#            SUM(sales) AS ttl_salse
#      FROM `prj-test3.bq_trial.null_treatment`
#      GROUP BY 1;
#      #| |shop_name|ttl_qty|ttl_salse|
#      #|1|新宿     |      5|    27800|
#      #|2|渋谷     |      4|    21800|


/********************************************************************/
# AVG() - 平均値
# eg. SELECT AVG(quantity) FROM `prj-test3.bq_sample.shop_purchases`;
#     #| |f0_              |
#     #|1|2.258190327613107|

# ※NULLの振る舞い(計算時は無視される)
# eg. SELECT AVG(quantity) AS avg_qty, AVG(sales) AS avg_salse
#     FROM `prj-test3.bq_trial.null_treatment`;
#     #| | avg_qty|avg_salse|
#     #|1|    2.25|  12400.0|

#【5.6 演習問題1(2:30)】
-- SELECT shop_id, AVG(quantity) AS avg_qty
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY shop_id
-- ORDER BY 2 DESC;
-- #| |shop_id|avg_qty           |
-- #|1|      5|               3.0|
-- #|2|      3|2.4468085106382977|

#【5.6 演習問題2(3:30)】
-- SELECT shop_id, AVG(sales_amount) AS avg_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE date BETWEEN "2018-06-01" AND "2018-06-30"
-- GROUP BY shop_id
-- ORDER BY 2 DESC;
-- #| |shop_id|avg_sales        |
-- #|1|      1|17558.62222222221|
-- #|2|      2|12881.13043478261|


/********************************************************************/
# ■ MAX(),MIN() - 最大値、最小値
# eg. SELECT MAX(sales_amount) AS max, MIN(sales_amount) AS min
#     FROM `prj-test3.bq_sample.shop_purchases`;
#    #| |max  |min |
#    #|1|99000|1400|

#【5.7 演習問題1(1:15)】
-- SELECT shop_id, MAX(sales_amount) AS max_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE product_id = 15
-- GROUP BY shop_id
-- ORDER BY 1;
-- #| |shop_id|max_sales|
-- #|1|      1|    19300|
-- #|2|      2|    20000|

#【5.7 演習問題2(3:15)】
-- SELECT date,
--        shop_id,
--        MIN(sales_amount)AS min_salse
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE product_id = 4 AND quantity = 1
-- GROUP BY 1, 2
-- ORDER BY 3;
-- #| |date      |shop_id|min_salse|
-- #|1|2018-06-30|      3|    13260|
-- #|2|2018-01-21|      3|    13650|


/********************************************************************/
# ■ 標準偏差（=standard deviation）
# 分析対象が母集団（=population）：SELECT STDDEV_POP(カラム名)
# 分析対象が標本（＝sample）     ：SELECT STDDEV_SAMP(カラム名)

#【5.8 演習問題1(2:35)】
-- SELECT shop_id,
--        STDDEV_POP(sales_amount) AS std_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE product_id = 15
-- GROUP BY shop_id
-- ORDER BY 2;
#| |shop_id|std_sales         |
#|1|      4|3664.3411301852543|
#|2|      1| 4962.301658504852|


/********************************************************************/
# ■ HAVING句 - 集計結果にフィルタリング
# eg. SELECT shop_id,
#            SUM(quantity) AS sum_qty
#     FROM `prj-test3.bq_sample.shop_purchases`
#     GROUP BY shop_id
#     HAVING SUM(quantity) > 1000 #←ココ
#     ORDER BY sum_qty DESC;
#     #| |shop_id|sum_qty|
#     #|1|      2|   1257|
#     #|2|      1|   1063|

# ※ WHERE句とHAAVING句の違い
#    ・WHERE句は、集計前に実行（絞り込み）される。
#    ・HAVING句は、集計後の結果に対して実行される。
#    似ているが混同しないように！
#
# eg. WHERE SUM(quantity) > 500
#     (error) Aggregate function SUM not allowed in WHERE clause

#【5.9 演習問題1(4:30)】
-- SELECT shop_id,
--        AVG(sales_amount) AS avg_salse
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE product_id = 18
-- GROUP BY shop_id
-- HAVING avg_salse > 15000
-- ORDER BY avg_salse DESC;
-- #| |shop_id|avg_slse          |
-- #|1|      4|           25090.0|
-- #|1|      2|16971.185185185186|

#【5.9 演習問題2(10:45)】
-- SELECT prefecture,
--        COUNT(user_id) AS users
-- FROM `prj-test3.bq_sample.customers`
-- WHERE Is_premium = true
-- GROUP BY 1
-- HAVING users <= 15
-- ORDER BY 2 DESC;
-- #| |prefecture|users|
-- #|1|Osaka     |   11|
-- #|2|Kanagawa  |   11|


/********************************************************************/
# ■ 各区の記述順序
# 1. SELECT    ：取得する列の指定
#    （集計関数）：
# 2. FROM      ：取得するテーブルの指定
# 3. WHERE     ：絞り込み条件の指定
# 4. GROUP BY  ：グループ化項目の指定
# 5. HAVING    ：グループ化された集計結果に対して絞り込み条件の指定
# 6. ORDER BY  ：並び替え条件
# 7. LIMIT     ：表示する行数の指定


/********************************************************************/
# ■ 各区の実行順序
# 1. FROM
# 2. WHERE
# 3. GROUP BY
# 4. (集計関数)
# 5. HAVING
# 6. SELECT
# 7. ORDER BY
# 8. LIMIT


```
<!-- ----------- section5 END ----------- -->

### ● Section6
```SQL
/**************************************************************
 * 【Uddemy】BigQueryで学ぶ非エンジニアのためのSQLデータ分析入門
 *  section : 6
 **************************************************************/

# ■ 四則演算
# eg. SELECT 3*10;
#     #| |f0_|
#     #|1| 30|
#    ∴ BigQueryは計算機能を有している。

# eg. 購入ID別の売上高と、税込価格を知りたい。
#     SELECT purchase_id,
#            sales_amount,
#            sales_amount * 1.08 AS sales_with_tax
#     FROM `prj-test3.bq_sample.shop_purchases`;
#     #| |purchase_id|sales_amount|sales_with_tax   |
#     #|1|         22|       59400|64152.00000000001|
#     #|2|        388|       59400|64152.00000000001|

#【6.2 演習問題1(2:40)】
-- (miss_code)
-- SELECT purchase_id,
--        quantity,
--        sales_amount,
--        AVR(sales_amount) AS avg_price　#[×]
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY purchase_id
-- ORDER BY 4 DESC;

-- SELECT purchase_id,
--        quantity,
--        sales_amount,
--        sales_amount/quantity AS avg_price
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY 4;
-- # | |purchase_id|quantity|sales_amount|avg_salse|
-- # |1|        301|       2|        2660|   1330.0|
-- # |2|       1040|       1|        1400|   1400.0|

#【6.2 演習問題2(4:10)】
-- (miss code)
-- SELECT purchase_id,
--        quantity,
--        sales_amount,
--        sales_amount/quantity AS avg_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- HAVING avg_sales > 5000　#[×]
-- ORDER BY 4;

-- SELECT purchase_id,
--        quantity,
--        sales_amount,
--        sales_amount/quantity AS avg_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE sales_amount/quantity > 5000
-- ORDER BY 4;
-- # | |purchase_id|quantity|sales_amount|avg_salse|
-- # |1|       1196|       2|       10030|   5015.0|
-- # |2|       1079|       1|        5015|   5015.0|


/******************************************************************
 * 数列に対する基本的な関数
 ******************************************************************/
# ■ ROOUND() - 数字を丸める関数
# ROUND(値、加算可能かラム名, 桁数)
# 戻り値はのデータ型は浮動小数（float）

# eg. SELECT
#         ROUND(154.249) AS basic,
#         ROUND(154.249, 1) AS Plus1,
#         ROUND(154.249, 2) AS Plus2,
#         ROUND(154.249, -1) AS Minus1,
#         ROUND(154.249, -2) AS Minus2;
#     | |basic|Plus1|Plus2 |Minus1|Minus2|
#     |1|154.0|154.2|154.25| 150.0| 200.0|


/********************************************************************/
# ■  CEIL() FLOOR() - 数字を丸める関数
#  切り上げ関数： CEIL()
#  切り捨て関数： FLOOR()
# ※ いずれも戻り値が浮動小数点（float）
#【6.3 演習問題1(4:00)】
-- SELECT purchase_id,
--        sales_amount,
--        sales_amount*1.08 AS Sales_w_tax,
--        ROUND(sales_amount*1.08, 1) AS Round,
--        CEIL(sales_amount*1.08) AS Ceil,
--        FLOOR(sales_amount*1.08) AS Floor
-- FROM `prj-test3.bq_sample.shop_purchases`
-- -- GROUP BY purchase_id, sales_amount    #[×]不要：but あっても同結果
-- ORDER BY purchase_id ASC
-- LIMIT 50;
-- #| |purchase_id|sales_id|Salse_w_tax|Round  |Ceil   |Floor  |
-- #|1|          1|  112775|    13797.0|13797.0|13797.0|13797.0|
-- #|2|          2|   11600|    12528.0|12528.0|12528.0|12528.0|

#【6.3 演習問題2(6:30)】
-- SELECT purchase_id,
--        sales_amount,
--        ROUND(sales_amount*1.08, -2) AS Sales_w_tax #十の位で四捨五入
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY purchase_id
-- LIMIT 50;
-- #| |purhase_id|salse_amount|Salse_w_tax|
-- #|1|         1|       12775|    13800.0|
-- #|2|         2|       11600|    12500.0|

#【6.3 演習問題3(8:00)】
-- SELECT purchase_id,
--        sales_amount,
--     --    CEIL(ROUND(sales_amount*1.08, -2)) AS Salse_w_tax  #[×]
--       CEIL(sales_amount*1.08/100)*100 AS Salse_w_tax #十の位で切り上げ
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY purchase_id
-- LIMIT 50;
-- #| |purhase_id|salse_amount|Salse_w_tax|
-- #|1|         1|       12775|    13800.0|
-- #|2|         2|       11600|    12600.0|

/********************************************************************/
# ■ TRUNC() -


/********************************************************************/
# ■ ABS() - 絶対値
#【6.4 演習問題1(1:30)
-- SELECT ROUND(AVG(sales_amount), 1) AS avg_salse
-- FROM `prj-test3.bq_sample.shop_purchases`;
-- #| |avg_salse|
-- #|1|  15693.0|

-- SELECT shop_id,
--        AVG(sales_amount) AS avg_salse,
--        AVG(sales_amount)-15693 AS diff,
--        ABS(AVG(sales_amount)-15693) AS abs_diff
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY shop_id
-- ORDER BY shop_id;
-- #| |shop_id|avg_salse         |diff               |abs_diff          |
-- #|1|      1| 15810.55632582321|  117.5563258232105| 117.5563258232105|
-- #|2|      2|15691.087794432557|-1.9122055674433796|1.9122055674433796|


/********************************************************************/
# ■ MOD() - 割り算の余り
#  MOD(割られる数, 割る数)
#【6.4 演習問題2(4:30)】
-- SELECT *
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE MOD(purchase_id, 3)=0
-- ORDER BY purchase_id;
-- #| |purchase_id|user_id|date      |shop_id|product_id|quantity|salse_amount|
-- #|1|          3| 876530|2018-01-01|      2|        19|       3|        4845|
-- #|2|          6| 954830|2018-01-01|      1|        17|       3|       15488|

#（用途メモ）数億レコードあるデータから、ザクっと間引きしてみたい時に用いる。（ランダム抽出）
#           データ量を1/3にしたり、1/5にしたりとする。


/********************************************************************/
# ■ CAST() - 型変換
# 整数（INT64）→文字列（STRNG）
# 少数（FLOAT64）→整数（INT64）
# eg. CAST({対象かラム} AS {変換先データ型})

#【6.4 演習問題3(8:00)】
-- SELECT purchase_id,
--        sales_amount,
--        CAST(ROUND(sales_amount*1.08, -2) AS INT64) AS sales_w_tax
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY purchase_id, sales_amount
-- ORDER BY 2 DESC
-- LIMIT 5;
-- #| |purchase_id|sales_amount|sales_tax|
-- #|1|       1203|       99000|   106900|
-- #|2|        995|       99000|   106900|


/******************************************************************
 * 文字列に対する基本的な関数
 ******************************************************************/
# ■ CONCAT() - 文字列連結
# eg. CONCAT({文字列}, {文字列}, {文字列}・・・)
#【6.5 演習問題1(1:50)】
-- SELECT user_id,
--        CONCAT(last_name, " ", first_name) AS full_name
-- FROM `prj-test3.bq_sample.customers`
-- ORDER BY 1;
-- #|  |user_id|full_name|
-- #| 1| 496070|片桐 雅友 |
-- #| 2| 496070|才村 歩加 |
-- #|21| 613645|#N-A 守  |

#【6.5 演習問題2(3:10)】
-- SELECT purchase_id,
--        CONCAT(CAST(user_id AS STRING), "-", CAST(shop_id AS STRING)) AS user_shop,
--        sales_amount
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY purchase_id
-- LIMIT 10;
-- #| |purchase_id|user_shop|sales_amount|
-- #|1|          1|733995-2 |       12775|


/********************************************************************/
# ■ LENGTH() - 文字列の文字数取得
#【6.5 演習問題3(5:30)】
-- SELECT product_id,
--        prod_name,
--        LENGTH(prod_name) AS letters
-- FROM `prj-test3.bq_sample.products_master`
-- ORDER BY 3;
-- #| |product_id|pod_name     |letters|
-- #|1|         9|ブラウス 半袖|      7|  ※半角スペースもカウント


/********************************************************************/
# ■ SUBSTR() - 文字列の一部取得
# eg. SUBSTR(prod_name, 1, 5)
#     = prod_nameの１番左から、5文字取得
#【6.5 演習問題4(8:00)】
-- SELECT product_id,
--        prod_name,
--        SUBSTR(prod_name, -6, 6) AS right6 #右から6文字
-- FROM `prj-test3.bq_sample.products_master`
-- ORDER BY product_id;
-- #| |product_id|prod_name                      |right6 |
-- #|1|         1|オックスフォードシャツ 綿 100%|綿 100%|


/********************************************************************/
# ■ REPLACE() - 文字列の一部置き換え
# eg. REPLACE("creating", "ing", "ed")
#     = ing(現在進行形) を ed(過去形) に置き換える
#【6.5 演習問題5(10:10)】
-- SELECT product_id,
--        prod_name,
--        REPLACE(prod_name, "半袖", "七分袖") AS prod_name
-- FROM `prj-test3.bq_sample.products_master`
-- ORDER BY product_id;
-- #| |product_id|prod_name    |prod_name_1   |
-- #|7|         7|ポロシャツ 半袖|ポロシャツ 七分袖|
-- #|8|         8|ポロシャツ 長袖|ポロシャツ 長袖 |


/********************************************************************/
# ■ TRIM() - 文字列の前後の空白、任意の文字数削除
# eg. TRIM({文字列}, {削除したい文字})
#     文字列前後の＄マークを削除したい。
#     ＝TRIM(文字列,"$")
#【6.5 演習問題6(12:00)】
-- SELECT " 平成 ", TRIM("平成");
-- #| |f0_  |f1_|
-- #|1| 平成 |平成|

-- SELECT "https://www.udemy.com",
--         TRIM("https://www.udemy.com", "https://");
-- #| |f0_                  |f1_          |
-- #|1|https://www.udemy.com|www.udemy.com|


/********************************************************************
# 正規表現関数（BigQueryには正規表現を利用できる関数がある。）
********************************************************************/
# ■ REGEXP_CONTAINS() - 指定正規表現の含有の有無
# eg. REGEXP_CONTAIN(文字列, r"正規表現")
#     文字列に？が入っているか否か確認。
#     SELECT
#        REGEXP_CONTAINS("/products/?id=15", r"\?"), #Windowsでは \ ではなく ¥
#        REGEXP_CONTAINS("/company/", r"\?");
#     #| |f0_ |f1_  |
#     #|1|true|false|
#【6.6 演習問題1(2:50)】
#(miss_code)
-- SELECT
--     REGEXP_CONTAINS(page, r"\?") IS TRUE AS prob_dir,
--     SUM(pageview) AS PV
-- FROM `prj-test3.bq_sample.web_usage`
-- GROUP BY 1;
-- #| |prob_dir|PV  |
-- #|1|false   | 653|
-- #|2|true    |1945|
#(collect code)
-- SELECT SUM(pageview) AS PV
-- FROM `prj-test3.bq_sample.web_usage`
-- WHERE REGEXP_CONTAINS(page, r"^/products/\?id=[0-9][0-9]?$") IS TRUE; #[0-9]:0~9数字が1つ、 ?：直前の文字があってもなくても良いの意味
-- #| |PV  |
-- #|1|1944|


/********************************************************************/
# ■ REGEXP_EXTRACT() - 指定正規表現の文字列を抽出
# eg. REGEXP_EXTRACT(value, regex [, position[, occurrence]])
#     SELECT
#        REGEXP_EXTRACT("sample@gmail.com", r"^[^@]+"),   #^:始めから、[^@]:＠でないところまで、+：1つ以上の繰り返す
#        REGEXP_EXTRACT("03-1234-5678", r"-(\d+)-"),      #-()-:ハイフンに囲まれた、\d:数字、+：1つ以上の繰り返す
#        REGEXP_EXTRACT("google / organic", r"/\s(.+)$"); #スラッシュの後に \s:半角スペース、(.+):どんな文字列でも1つ以上の繰り返されたもの、$:最後まで
#     #| |f0_   |f1_ |f2_    |
#     #|1|sample|1234|organic|


/********************************************************************/
# ■ REGEXP_REPLACE() - 置き換え
# eg. REGEXP_REPLACE(value, regex, replacement)
#     SELECT
#        REGEXP_REPLACE("abc@gmail.com", r"a.c", "XYZ"),   #[a]と、[.]:何でもいいと、[c]の文字列
#        REGEXP_REPLACE("bbc@gmail.com", r"a.c", "XYZ"),
#        REGEXP_REPLACE("abc@gmail.com", r"[^@]", "X"),    # [^@]:＠マークでない全ての文字
#        REGEXP_REPLACE("www.sample.com/products/", r"(^.+\.com)", "sample.jp"),
#        REGEXP_REPLACE("www.sample.ac.jp/products/", r"(^.+\.com)", "sample.jp");
#     #| |f0_          |f1_          |f2_          |f3_                |f4_                       |
#     #|1|XYZ@gmail.com|bbc@gmail.com|XXX@XXXXXXXXX|sample.jp/products/|www.sample.ac.jp/products/|


/******************************************************************
 * 日時、日付に対する基本的な関数
 ******************************************************************/
# ■ DATE()、DATETIME() - DATE型(DATETIME型)
# eg. DATE(2021, 9, 30)#2021/09/30
#     DATETIME(2021,9,30,12,35,15) #2021/09/30 12:35:15
#【6.7 演習問題1(1:50)】
-- SELECT
--     DATE(2019, 4, 30) AS date,
--     DATETIME(2019, 4, 30, 13, 22, 15) AS datetime;
-- #| |date      |datetime           |
-- #|1|2019-04-30|2019-04-30T13:22:15|

-- # 実行後 -> ジョブ情報（結果画面横）-> 一時テーブル（ページ最下部）-> テーブルスキーマーを確認。
-- #|フィールド名|種類     |モード   |
-- #|date      |DATE    |NULLABLE|
-- #|datetime  |DATETIME|NULLABLE|

#【6.7 演習問題2(4:30)】
-- SELECT
--     purchase_id,
--     DATETIME(date) AS datetime
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY purchase_id
-- LIMIT 10;
-- #| |purchase_id|datetime           |
-- #|1|          1|2018-01-01T00:00:00|

#【6.7 演習問題3(5:30)】
-- SELECT
--     timestamp,
--     DATE(timestamp) AS date
-- FROM `prj-test3.bq_sample.web_usage`
-- ORDER BY 1
-- LIMIT 100;
-- #| |timestamp          |date      |
-- #|1|2018-01-01T13:12:57|2018-01-01|


/********************************************************************/
# ■ CURRENT_DATE()、CURRENT_DATETIME() - 現在データの取得
# ※現在時刻をUTC（協定世界時：日本より9時間前）で取得する。
# eg. SELECT
#        CURRENT_DATE("+09:00"),
#        CURRENT_DATETIME("+09:00");
#【6.7 演習問題5(9:15)】
-- SELECT
--     CURRENT_DATE("Asia/Tokyo"),
--     CURRENT_DATETIME("Asia/Tokyo");


/********************************************************************/
# ■ DATE_ADD()、DATETIME_ADD() - 追加
# DATE_ADD(date_expr, INTERVAL int64_expr date_part)
# eg. 2019年5月3日に３週間を加算する。
#     DATE_ADD(date"2019-05-03", INTERVAL 3 week)
#
# ※ DATE: date_part = [yera, quarter, month, week, day]
# ※ DATETIME: date_part = [yera, quarter, month, week, day]
#                         +[hour, minute , second, milisecond, microsecond] 細かく指定が可能。
#【6.8 演習問題1(2:15)】
-- SELECT DATE_ADD(CURRENT_DATE("Asia/Tokyo"), INTERVAL 3 month);
-- #||f0_       |
-- #|1|2021-12-6|
# (memo) 例えば、3ヶ月前とかも取れる。その場合は「-3」

#【6.8 演習問題2(3:50)】
-- SELECT DATETIME_ADD(CURRENT_DATETIME("+9:00"), INTERVAL 6 hour);
-- #| |f0_                       |
-- #|1|2021-09-07T04:21:28.704252|


/********************************************************************/
# ■ DATE_SUB()、DATETIME_SUB() - 差し引く
# eg. SELECT DATE_SUB(date_expr, INTERVAL int64_expr date_part);


/********************************************************************/
# ■ DATE_DIFF()、DATETIME_DIFF() - 差分
# eg. SELECT DATE_DIFF(date_expr, date_expr, date_part);
#【6.8 演習問題3(6:30)】
-- SELECT DATE_DIFF(date"2019-04-30", date"1989-01-08", day);
-- #| |f0_  |
-- #|1|11069|

#【6.8 演習問題4(7:30)】
-- SELECT DATETIME_DIFF(datetime"2019-05-02T10:12:15", datetime"2019-05-02T09:45:23", second);
-- #| |f0_ |
-- #|1|1612|


/********************************************************************/
# ■ DATE_TRUNC()DATETIME_TRUNC() - 切り詰める（=Truncate）
# eg. SELECT DATE_TRUNC(date_expr, date_part)
#     SELECT DATE_TRUNC("1989-01-15", month);
#      #| |f0_       |
#      #|1|1989-01-01|←月以降は切り詰められている
#
#     SELECT DATETIME_TRUNC("1989-05-02 06:39:25", day);
#      #| |f0_               |
#      #|1|1989-05-02 0:00:00| ←日付以降は切り詰められている
#
#【6.9 演習問題1(2:10)】
-- SELECT
--     DATE_TRUNC(date, MONTH) AS month,
--     SUM(sales_amount) AS total_sales
-- FROM  `prj-test3.bq_sample.shop_purchases`
-- GROUP BY 1
-- ORDER BY 1;
-- #| |month     |total_sales|
-- #|1|2018-01-01|    2056764|
-- #|2|2018-02-01|    1133128|


/********************************************************************/
# ■ FORMAT_DATE()FORMAT_DATETIME() - 出力フォーマット
# eg. SELECT FORMAT_DATE(format_string, date);
#        SELECT
#            FORMAT_DATE("%F", "2019-05-02") AS F_upper,
#            FORMAT_DATE("%x", "2019-05-02") AS x_lower,
#            FORMAT_DATE("%Y", "2019-05-02") AS Y_upper,
#            FORMAT_DATE("%y", "2019-05-02") AS y_lower,
#            FORMAT_DATE("%m", "2019-05-02") AS m_lower,
#            FORMAT_DATE("%d", "2019-05-02") AS d_lower,
#            FORMAT_DATE("%A", "2019-05-02") AS A_upper,
#            FORMAT_DATE("%a", "2019-05-02") AS a_lower,
#            FORMAT_DATE("%B", "2019-05-02") AS B_upper,
#            FORMAT_DATE("%b", "2019-05-02") AS b_lower;
#        #| |F_upper   |x_lower |Y_upper|y_lower|m_lower|d_lower|A_upper |a_lower|B_upper|b_lower|
#        #|1|2019-05-02|05/02/19|   2019|     19|     05|     02|Thursday|Thu    |May    |May    |
#
#        SELECT
#            FORMAT_DATETIME("%c", "2019-05-02 09:29:41") AS c_lower,
#            FORMAT_DATETIME("%r", "2019-05-02 09:29:41") AS r_lower,
#            FORMAT_DATETIME("%X", "2019-05-02 09:29:41") AS X_upper,
#            FORMAT_DATETIME("%H", "2019-05-02 09:29:41") AS H_upper,
#            FORMAT_DATETIME("%M", "2019-05-02 09:29:41") AS M_upper,
#            FORMAT_DATETIME("%S", "2019-05-02 09:29:41") AS S_upper,
#            FORMAT_DATETIME("%P", "2019-05-02 09:29:41") AS P_upper,
#            FORMAT_DATETIME("%s", "2019-05-02 09:29:41") AS s_lower,
#            FORMAT_DATETIME("%p", "2019-05-02 09:29:41") AS p_lower;
#        #| |c_lower                |r_lower    |X_upper |H_upper|M_upper|S_upper|P_upper|s_lower   |p_lower|
#        #|1|Thu May 2 09:29:41 2019|09:29:41 AM|09:29:41|09     |29     |41     |am     |1556789381|AM     |


/********************************************************************/
# ■ MAX()MIN() - 最大値、最小値
#【6.9 演習問題2(4:00)】
-- SELECT
--     shop_id,
--     -- MIN(DATE_TRUNC(date, DAY)) AS oldest,
--     -- MAX(DATE_TRUNC(date, DAY)) AS newest,
--     -- MAX(DATE_TRUNC(date, DAY))-MIN(DATE_TRUNC(date, DAY)) AS days_diff
--     MIN(date) AS oldest,
--     MAX(date) AS newest,
--     DATE_DIFF(MAX(date), MIN(date), DAY) AS days_diff
-- FROM `prj-test3.bq_sample.shop_purchases`
-- WHERE shop_id = 4
-- GROUP BY shop_id;
-- #| |shop_id|oldest    |newest    |days_diff|
-- #|1|      4|2018-01-05|2018-12-19|      348|

#【6.9 演習問題3(9:00)】
#(miss code)
-- SELECT
--     user_id,
--     MIN(DATETIME_TRUNC(timestamp, DAY)) AS oldest,
--     MAX(DATETIME_TRUNC(timestamp, DAY)) AS newest,
--     DATETIME_DIFF(
--         MAX(DATETIME_TRUNC(timestamp, DAY)),
--         MIN(DATETIME_TRUNC(timestamp, DAY)),
--         DAY) AS days_diff,
--     session_count
--     -- AVG() AS avg_visit
-- FROM `prj-test3.bq_sample.web_usage`
-- GROUP BY user_id, session_count
-- HAVING DATETIME_DIFF(
--         MAX(DATETIME_TRUNC(timestamp, DAY)),
--         MIN(DATETIME_TRUNC(timestamp, DAY)),
--         DAY) > 1
-- ORDER BY days_diff;
#(collect code)
-- SELECT
--     user_id,
--     MIN(timestamp) AS oldest,
--     MAX(timestamp) AS newest,
--     DATETIME_DIFF(MAX(timestamp), MIN(timestamp), DAY) AS days_diff,
--     COUNT(DISTINCT session_count) AS visit_count, #[重複session数を除いて、ユニークなsession数を取得]
--     DATETIME_DIFF(MAX(timestamp), MIN(timestamp), DAY)/COUNT(DISTINCT session_count) AS avg_interval
-- FROM `prj-test3.bq_sample.web_usage`
-- GROUP BY user_id
-- HAVING
--     DATETIME_DIFF(MAX(timestamp), MIN(timestamp), DAY) <> 0
--     AND COUNT(DISTINCT session_count) <> 1
-- ORDER BY avg_interval;
-- #| |user_id|oldest             |newest             |days_diff|visit_count|avg_interval|
-- #|1| 916550|2018-10-22T00:46:50|2018-10-23T14:34:55|        1|          8|       0.125|
-- #|2|1011670|2018-10-04T18:37:15|2018-10-05T13:42:21|        1|          4|        0.25|

/********************************************************************
 * 条件式
 ********************************************************************/
# ■ IF文 - 条件式
# eg. IF(条件式, True, False)
#
#    SELECT
#        product_id,
#        prod_name,
#        IF(product_id >= 11, "Tシャツ", "それ以外") AS category
#    FROM `prj-test3.bq_sample.products_master`;
#    #|  |product_id|prod_name                 |category|
#    #|1 |         1|オックスフォードシャツ 綿 100%|それ以外 |
#    #|11|        11|Tシャツ（デザイン） 半袖      |Tシャツ  |
#【6.10 演習問題1(3:20)】
-- SELECT
--     IF(product_id >= 11, "Tシャツ", "Tシャツ以外") AS category,
--     SUM(quantity) AS total_pty
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY 1;
-- #| |category  |total_qty|
-- #|1|Tシャツ以外 |     1156|
-- #|2|Tシャツ    |     1739|


/********************************************************************/
# ■ IFNULL文 - nullに対しての条件式
# eg. IFNULL(かラム名, nullの場合の返り値)
# ※ 値がnullでないなら何もしない。
#【6.10 演習問題2(5:20)】
-- SELECT
--     *,
--     IFNULL(gender, 3) AS gender_not_null
-- FROM `prj-test3.bq_sample.customers`
-- ORDER BY 1;
-- #| |user_id||gender||gender_not_null|
-- #|1| 496070||     1||              1|
-- #|7| 596992||null  ||              3|


/********************************************************************/
# ■ CASE文 - 条件式
# 構成 = CREATE,WHEN,THEN,ELSE,END
# ※ 分析では頻繁に用いる関数！
# eg.
#    CASE (判定条件)  #←Bool判定式では記載無いこともある
#       WHEN 値 THEN 値が合致した場合の戻り値
#       WHEN 値 THEN 値が合致した場合の戻り値
#       WHEN 値 THEN 値が合致した場合の戻り値
#       ELSE いずれにも合致しなかった場合の戻り値
#    END
#
#    SELECT
#        user_id,
#        gender,
#        CASE gender
#            WHEN 1 THEN "男性"
#            WHEN 2 THEN "女性"
#            ELSE "不明"
#        END AS gender_name
#    FROM `prj-test3.bq_sample.customers`
#    ORDER BY 1;
#    #| |user_id|gender|gender_name|
#    #|1| 496070|     1|男性        |
#    #|2| 497520|     2|女性        |
#    #|7| 596992|null  |不明        |
#
#    SELECT
#        user_id,
#        prefecture,
#        CASE
#            WHEN REGEXP_CONTAINS(prefecture, r"Tokyo|Kanawa") THEN "東京他" # |:または
#            WHEN REGEXP_CONTAINS(prefecture, r"Osaka|Hyogo") THEN "大阪他"
#            ELSE "その他"
#        END AS pref_category
#    FROM `prj-test3.bq_sample.customers`
#    ORDER BY 1;
#    #| |user_id|prefecture|pref_category|
#    #|1| 496070|Tokyo     |東京他        |
#    #|4| 529710|Fukuoka   |その他        |
#    #|9| 597427|Hyogo     |大阪他        |
#【6.11 演習問題1(6:30)】
-- SELECT
--     sales_amount,
--     ROUND(sales_amount, -3)
-- FROM `prj-test3.bq_sample.shop_purchases`;
#(miss_code)
-- SELECT
--     purchase_id,
--     sales_amount,
--     CASE
--         WHEN ROUND(sales_amount, -3) = 0 THEN "１万円未満"
--         WHEN ROUND(sales_amount, -3) = 10000 THEN "１万円"
--         WHEN ROUND(sales_amount, -3) = 20000 THEN "２万円"
--         ELSE "３万円以上"
--     END AS round_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY 1;
#(collect code)
-- SELECT
--     purchase_id,
--     sales_amount,
--     CASE ROUND(sales_amount, -4)
--         WHEN 0 THEN "１万円未満"
--         WHEN 10000 THEN "１万円"
--         WHEN 20000 THEN "２万円"
--         ELSE "３万円以上"
--     END AS round_sales
-- FROM `prj-test3.bq_sample.shop_purchases`;
-- #|  |purchase_id|sales_amount|round_sales|
-- #| 1|         22|       59400|３万円以上   |
-- #| 5|         17|       19000|２万円      |
-- #|74|        335|       12580|１万円      |

#【6.11 演習問題2(11:30)】
#(miss_code)
-- SELECT
--     CASE product_id
--         WHEN 1 BETWEEN 6 THEN "男性衣類"
--         WHEN 7 BETWEEN 10 THEN "女性衣類"
--         WHEN 11 BETWEEN 18 THEN "Tシャツ"
--         WHEN IN(19, 20) THEN "子供用Tシャツ"
--         ELSE "不明"
--     END AS prod_category,
--     AVG(sales_amount) AS avg_amount
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY prod_category
-- ORDER BY 2;
#(colect code)
-- SELECT
--     CASE
--         WHEN product_id <= 6 THEN "男性衣類"
--         WHEN product_id <= 10 THEN "女性衣類"
--         WHEN product_id <= 18 THEN "Tシャツ"
--         ELSE "子供用Tシャツ"
--     END AS prod_category,
--     ROUND(AVG(sales_amount), 2)AS avg_amount
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY 1
-- ORDER BY 2 DESC;
-- #| |prod_category|avg_amount|
-- #|1|男性衣類      |  30074.72|
-- #|2|女性衣類      |  24052.13|

```
<!-- ----------- section6 END ----------- -->

### ● Section7
```SQL
/**************************************************************
 * 【Uddemy】BigQueryで学ぶ非エンジニアのためのSQLデータ分析入門
 *  section : 7
 **************************************************************/

/**************************************************************
# 分析関数
**************************************************************/

# ■ OVER句 - 分析関数の書式
# ※分析において最も重要な句。内側で3つの(option)がある。
#    OVER(
#       PARTITION BY [分析を行うグループを作る対象のカラム名](option)
#       ORDER BY [パーテション中での並べ替えを行うカラム名] ASC|DESC (option)
#       WINDOW_FRAME(option)
#    ) AS 別名
#
# eg. SELECT
#         user_id,
#         order_id,
#         quantity,
#         SUM(quantity)OVER(
#             PARTITION BY user_id         --PARTITION BY句： user_id別にまとめる
#             ORDER BY quantity DESC       --ORDER BY句：patation内でquantityが多い順
#             ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW    --WINDOW_FRAME句: ROWSを指定（他にもRANGEがある）
#         ) AS cumlative_quantity_till_the_row
#     FROM `prj-test3.bq_trial.pos`
#     GROUP BY user_id, order_id, quantity;
#                       --〈desc〉--
#    #| |user_id|order_id|quantity|cumlative_quantity_till_the_row| --境界の無い当該行までの累計数量取得
#    #|1|ABC    |       9|      12|                             12|
#    #|2|ABC    |       1|      10|                             22|←(12+10)
#    #|3|ABC    |       3|       8|                             30|←(12+10+8)
#    #|4|ABC    |      10|       6|                             36|←(12+10+8+6)
#    #|5|ABC    |       2|       5|                             41|←(12+10+8+6+5)
#    #-----------------------------------------------------------------------〈patition〉
#    #|6|STU    |      12|      12|                             12|
#    #|7|STU    |       4|       8|                             20|←(12+8)
#    #|8|STU    |      11|       3|                             23|←(12+8+3)
#    #-----------------------------------------------------------------------〈patition〉
#    #|9|www    |       5|       5|                              5|

# ■ 番号付け関数
# RANK(): パーテションの中の順位を返す
# ROW_NUMBER() : パーテションの中の行数を返す

# ■ ナビゲーション関数
# FIRST_VALUE(): ウィンドウフレームの中の一番最初の値を返す
# LAST_VALUE(): ウィンドウフレームの中の一番最後の値を返す
# LEAD(): 1行、もしくは指定した行数後ろのレコーその値を返す。
# LAG(): 1行、もしくは指定した行数前のレコーその値を返す。

# ■ 集計分析関数
# SUM(): パーテションの中の合計値を返す
# AVG(): パーテションの中の平均値を返す
# MAX(): パーテションの中の最大値を返す
# MIN(): パーテションの中の最小値を返す

# ■ WINDOW_FRAME
# eg. [ROWS] [BETWEEN] UNBOUNDED PRECEDING [AND] CURRENT ROW #
# ※ BETWEEN句の始値、終値には様々な指定ができる。
#
# CURRENT ROW         : 現在の行
# UNBOUNDED PRECEDING : パーテションで定義された境界の、最も上の方
# UNBOUNDED FOLLOWING : パーテションで定義された境界の、最も下の方
# [int] PRECEDING     : 現在の行から[int]だけ上の行
# [int] FOLLOWING     : 現在の行から[int]だけ下の行
#
# eg. BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
#     → パーテション全体
# eg. BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
#     → パーテション最上部から現在の行まで。(SUMと一緒に使うと累計に)
# eg. BETWEEN 6 PRECEDING AND CURRENT ROW
#     → 現在行を含む、直上の7レコード。（ORDERBYを日付の昇順として、AVGと共に利用すると直近7日間の移動平均が取得できる。）





```
<!-- ----------- section7 END ----------- -->

### ● Section8
```SQL

```
<!-- ----------- section8 END ----------- -->

### ● Section9
```SQL

```
<!-- ----------- section9 END ----------- -->

### ● Section10
```SQL

```
<!-- ----------- section10 END ----------- -->

### ● Section11
```SQL

```
<!-- ----------- section11 END ----------- -->

### ● Section12
```SQL

```
<!-- ----------- section12 END ----------- -->

## || 参考
* [![img](https://img-c.udemycdn.com/course/240x135/2394060_adbb_4.jpg)](https://www.udemy.com/share/102kOc3@Jm55eXaV2GdLXwnNAoEOPHhXUleiQK0EG6JQboecG715rn2_tpL6jBbg8kL1nsqw/)<br>
[BigQuery で学ぶ非エンジニアのための SQL データ分析入門 - Udemy ](https://www.udemy.com/share/102kOc3@Jm55eXaV2GdLXwnNAoEOPHhXUleiQK0EG6JQboecG715rn2_tpL6jBbg8kL1nsqw/)

* [BigQuery で INFORMATION_SCHEMA から CREATE TABLE 文が取得できるようになりました！ - DevelopersIO](https://dev.classmethod.jp/articles/bigquery-information-schema-get-create-table-ddl/)
* [[初心者向け] Google BigQueryの基礎を理解してGoogle Cloud Consoleから触ってみた - DevelopersIO](https://dev.classmethod.jp/articles/google-bigquery-debut/)
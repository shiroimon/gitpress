---
date   : 2021-09-04
title  : 【BigQuery】分析入門 - Section5
excerpt: Google BigQuery基本の「き」について。
tags   : ["Google BigQuery", "SQL基本", "分析基本"]
---

## || Section5

### ■ COUNT() - 行数集計
e.g.
```SQL
# SELECT COUNT(*) FROM sample.shop_purchases;

/*「個数が１のレコード数をカウントしたい」※行数のカウントであってデータ個数ではない*/
SELECT COUNT(*)
FROM sample.shop_purchases
WHERE quantity = 1;
```
ex.
```SQL
#【5.2 演習問題1(2:09)】
SELECT COUNT(*) AS gyou_su
FROM `prj-test3.bq_sample.customers`
WHERE gender = 2 AND birthday >= "1989-01-08";
```
---
### ■ COUNT() - データ個数集計
e.g.
```SQL
# SELECT COUNT(かラム名)

/*「会員の名前を重複なく取得」カテゴリカル変数の分類集計*/
SELECT COUNT(DISTONCT first_name)
```
ex.
```SQL
#【5.2 演習問題2(6:50)】
SELECT COUNT(*) AS gyou_su,
       COUNT(first_name) AS data_kosu,
       COUNT(DISTINCT first_name) AS shurui_su
FROM `prj-test3.bq_sample.customers`
WHERE gender = 2;
```
---
### ■ GROUPBY句 - グループ化
```
データから意味を読み取るには。
「質的データ（定性）」項目でグループを作る。「量的データ（定量）」を集計する。
```
e.g. 店舗ID別のレコード数が知りたい。
```sql
SELECT shop_id,
       COUNT(*) AS No_of_purchase
FROM `prj-test3.bq_sample.shop_purchases`
WHERE date BETWEEN "2018-01-01" AND "2018-06-30"
GROUP BY shop_id      --店舗IDでまとめる(※SELECTに書いてないかラム名だとerror)
ORDER BY shop_id
LIMIT 5;
```
| |shop_id|No_of_purchase|
|-|-:|-:|
|1|      1|           265|
|2|      2|           207|
|3|      3|            87|

ex.
```SQL
#【5.4 演習問題1(3:30)】
SELECT product_id, COUNT(*) AS No_of_purchase
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY product_id
ORDER BY NO_of_purchase DESC;
-- | |product_id|No_of_purchase|
-- |1|        11|           102|
-- |2|        10|            98|
-- |3|        13|            93|


#【5.4 演習問題2(6:20)】
SELECT prefecture,
       COUNT(*) AS no_of_menbers
FROM `prj-test3.bq_sample.customers`
GROUP BY 1
ORDER BY 2 DESC;
-- | |prefecture|no_of_menbers|
-- |1|Tokyo     |          582|
-- |2|Osaka     |           88|
-- |3|Kanagawa  |           72|


#【5.4 演習問題3(8:00)】
# (miss_code)
-- SELECT shop_id,
--        COUNT(shop_id) AS No_of_purchases_by_shop,
--        COUNT(product_id) AS No_of_purchases_by_product
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY shop_id
-- ORDER BY 2 >= 3;

# (collect_code)
SELECT shop_id,
       product_id,
       COUNT(*) AS NO_of_purchase
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY shop_id, product_id
ORDER BY shop_id, product_id;
-- | |shop_id|product_id|No_of_purchase|
-- |1|      1|         1|            17|
-- |2|      1|         2|            17|
-- |3|      1|         3|            10|
```
```
（重要）
グルーピングにはSELECT内に記載した順番が問われる。
∴GROPBYの順番を変更したとしても問題はない。
```
---
### ■ SUM() - 合計値
e.g.
```SQL
SELECT SUM(quantity) FROM bq_sample.shop_purchases;
```
| |f0_ |
|-|-:|
|1|2895|

ex.
```SQL
#【5.5 演習問題1(1:56)】
SELECT shop_id, SUM(quantity) AS Total_quantity
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY 1
ORDER BY 2 DESC;
-- #| |shop_id|Taotal_quantity|
-- #|1|      1|           1257|
-- #|2|      2|           1063|
-- #|3|      3|            460|


#【5.5 演習問題2(3:30)】
SELECT shop_id, SUM(sales_amount) AS Total_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id IN(1, 5, 11, 18)
GROUP BY shop_id
ORDER BY Total_sales DESC;
-- #| |shop_id|Total_salse|
-- #|1|      1|    1789674|
-- #|2|      2|    1422230|
```
eg .※ `SUM()` 集計時はNULLが無視される。
```SQL
SELECT *
FROM `prj-test3.bq_trial.null_treatment`
ORDER BY 1;
```
```SQL
SELECT
    SUM(quantity) AS ttl_qty,
    SUM(sales) AS ttl_salse
FROM `prj-test3.bq_trial.null_treatment`;
```
| |ttl_qty|ttl_salse|
|-|-:|-:|
|1|      9|    49600|

```SQL
SELECT shop_name,
       SUM(quantity) AS ttl_qty,
       SUM(sales) AS ttl_salse
FROM `prj-test3.bq_trial.null_treatment`
GROUP BY 1;
```
| |shop_name|ttl_qty|ttl_salse|
|-|:-|-:|-:|
|1|新宿     |      5|    27800|
|2|渋谷     |      4|    21800|

---
### ■ AVG() - 平均値
eg.
```SQL
SELECT AVG(quantity) FROM `prj-test3.bq_sample.shop_purchases`;
```
| |f0_              |
|-|-:|
|1|2.258190327613107|

eg. ※ `AVG()` の集計時のNULLの振る舞い(計算時は無視される)
```SQL
SELECT
    AVG(quantity) AS avg_qty,
    AVG(sales) AS avg_salse
FROM `prj-test3.bq_trial.null_treatment`;
```
| | avg_qty|avg_salse|
|-|-:|-:|
|1|    2.25|  12400.0|

```SQL
#【5.6 演習問題1(2:30)】
SELECT shop_id, AVG(quantity) AS avg_qty
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY shop_id
ORDER BY 2 DESC;
-- #| |shop_id|avg_qty           |
-- #|1|      5|               3.0|
-- #|2|      3|2.4468085106382977|

#【5.6 演習問題2(3:30)】
SELECT shop_id, AVG(sales_amount) AS avg_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE date BETWEEN "2018-06-01" AND "2018-06-30"
GROUP BY shop_id
ORDER BY 2 DESC;
-- #| |shop_id|avg_sales        |
-- #|1|      1|17558.62222222221|
-- #|2|      2|12881.13043478261|
```

---
### ■ MAX(), MIN() - 最大値、最小値
e.g.
```SQL
SELECT
    MAX(sales_amount) AS max,
    MIN(sales_amount) AS min
FROM `prj-test3.bq_sample.shop_purchases`;
```
| |max  |min |
|-|-:|-:|
|1|99000|1400|

ex.
```SQL
#【5.7 演習問題1(1:15)】
SELECT shop_id, MAX(sales_amount) AS max_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id = 15
GROUP BY shop_id
ORDER BY 1;
-- #| |shop_id|max_sales|
-- #|1|      1|    19300|
-- #|2|      2|    20000|

#【5.7 演習問題2(3:15)】
SELECT date,
       shop_id,
       MIN(sales_amount)AS min_salse
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id = 4 AND quantity = 1
GROUP BY 1, 2
ORDER BY 3;
-- #| |date      |shop_id|min_salse|
-- #|1|2018-06-30|      3|    13260|
-- #|2|2018-01-21|      3|    13650|
```

---
### ■ 標準偏差（=standard deviation）
```
分析対象が母集団（= population）：STDDEV_POP(カラム名)
分析対象が標本（＝sample）     ：STDDEV_SAMP(カラム名)
```
ex.
```SQL
#【5.8 演習問題1(2:35)】
SELECT shop_id,
       STDDEV_POP(sales_amount) AS std_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id = 15
GROUP BY shop_id
ORDER BY 2;
-- #| |shop_id|std_sales         |
-- #|1|      4|3664.3411301852543|
-- #|2|      1| 4962.301658504852|
```

---
### ■ HAVING句 - 集計結果にフィルタリング
e.g.
```SQL
SELECT shop_id,
       SUM(quantity) AS sum_qty
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY shop_id
HAVING SUM(quantity) > 1000 --ココ
ORDER BY sum_qty DESC;
```
| |shop_id|sum_qty|
|-|-:|-:|
|1|      2|   1257|
|2|      1|   1063|

```
※ WHERE句とHAAVING句の違い
   ・WHERE句は、集計前に実行（絞り込み）される。
   ・HAVING句は、集計後の結果に対して実行される。
   似ているが混同しないように！

# eg. WHEREで同様な絞り込みをかけると、エラーが返る。
#      WHERE SUM(quantity) > 1000
#     (error) Aggregate function SUM not allowed in WHERE clause
```
ex.
```SQL
#【5.9 演習問題1(4:30)】
SELECT shop_id,
       AVG(sales_amount) AS avg_salse
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id = 18
GROUP BY shop_id
HAVING avg_salse > 15000
ORDER BY avg_salse DESC;
-- #| |shop_id|avg_slse          |
-- #|1|      4|           25090.0|
-- #|1|      2|16971.185185185186|

#【5.9 演習問題2(10:45)】
SELECT prefecture,
       COUNT(user_id) AS users
FROM `prj-test3.bq_sample.customers`
WHERE Is_premium = true
GROUP BY 1
HAVING users <= 15
ORDER BY 2 DESC;
-- #| |prefecture|users|
-- #|1|Osaka     |   11|
-- #|2|Kanagawa  |   11|
```

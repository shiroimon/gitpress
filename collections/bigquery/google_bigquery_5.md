---
date    : 2021-09-13
title   : 🔍 分析入門 Section5: 集計関数
excerpt : ---
tags    : ["🔍", "BigQuery", "Udemy"]
---


![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)


## || Section5
### | COUNT(*) - 行数集計
e.g.
```SQL
# SELECT COUNT(*) FROM sample.shop_purchases;

/*「個数が１のレコード数をカウントしたい」
※行数のカウントであってデータ個数ではない*/
SELECT COUNT(*) AS No_of_record
FROM sample.shop_purchases
WHERE quantity = 1;
```
ex.【5.2 演習問題1(2:09)】

customersテーブルから、gender=2、かつ、birthdayが1989年1月8日以降のレコード数を取得（平成生まれの女性の顧客数）。列名gyou_suとする。
```SQL
SELECT COUNT(*) AS gyou_su
FROM `prj-test3.bq_sample.customers`
WHERE
    gender = 2
    AND
    birthday >= "1989-01-08";

-- | |gyou_su|
-- |1||
```

### | COUNT(column) - データ個数集計
e.g.
```SQL
# SELECT COUNT(かラム名)

/*「会員の名前を重複なく取得」カテゴリカル変数の分類集計*/
SELECT COUNT(DISTONCT first_name)
```
ex.【5.2 演習問題2(6:50)】

customersテーブルから、女性の行数、女性のfirst_nameの個数、女性のfirst_nameの種類数を取得。列名は其々、gyou_su、data_kosu、shurui_su。
```SQL
SELECT
    COUNT(*) AS gyou_su,
    COUNT(first_name) AS data_kosu,
    COUNT(DISTINCT first_name) AS shurui_su
FROM `prj-test3.bq_sample.customers`
WHERE gender = 2;

-- | |gyou_su|data_kosu|shurui_su|
-- |1|    592|      588|      565|
```

### | GROUPBY句 - グループ化
```
データから意味を読み取るには。
  「質的データ（定性）」項目でグループを作る。
  「量的データ（定量）」を集計する。
```
e.g. 店舗ID別のレコード数が知りたい。
```sql
SELECT
    shop_id,
    COUNT(*) AS No_of_purchase
FROM `prj-test3.bq_sample.shop_purchases`
WHERE date BETWEEN "2018-01-01" AND "2018-06-30"
GROUP BY shop_id --店舗IDでまとめる (※SELECTに書いてないかラム名だとerror)
ORDER BY shop_id
LIMIT 5;
```
| |shop_id|No_of_purchase|
|-|-:     |-:            |
|1|      1|           265|
|2|      2|           207|
|3|      3|            87|

ex.【5.4 演習問題1(3:30)】

shop_purchasesテーブルから、product_idごとの販売件数（＝レコード数）を取得。
販売件数はNo_of_purchaseと別名にする。出力はNo_of_purchase降順。
```SQL
SELECT
    product_id,
    COUNT(*) AS No_of_purchase
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY product_id
ORDER BY NO_of_purchase DESC;

-- | |product_id|No_of_purchase|
-- |1|        11|           102|
-- |2|        10|            98|
-- |3|        13|            93|
```
ex.【5.4 演習問題2(6:20)】

customersテーブルから、prefectureごとの会員数を求める。結果は会員数の多い順に並べ替える。ただし、ORDERBY、GROUPBY何も列番号指定で記述。
```SQL
SELECT
    prefecture,
    COUNT(*) AS no_of_menbers
FROM `prj-test3.bq_sample.customers`
GROUP BY 1
ORDER BY 2 DESC;

-- | |prefecture|no_of_menbers|
-- |1|Tokyo     |          582|
-- |2|Osaka     |           88|
-- |3|Kanagawa  |           72|
```
ex.【5.4 演習問題3(8:00)】

shop_purchasesテーブルで、第一項目をshop_id第二項目をproduct_idとしてグループ化した販売件数（＝レコード数）をまとめ、shop_idの昇順、product_idの昇順の優先順位でソートして出力する。
```SQL
# (miss_code)
-- SELECT shop_id,
--        COUNT(shop_id) AS No_of_purchases_by_shop,
--        COUNT(product_id) AS No_of_purchases_by_product
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY shop_id
-- ORDER BY 2 >= 3;

# (collect_code)
SELECT
    shop_id,
    product_id,
    COUNT(*) AS NO_of_purchase
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY
    shop_id,
    product_id
ORDER BY
    shop_id,
    product_id;

-- | |shop_id|product_id|No_of_purchase|
-- |1|      1|         1|            17|
-- |2|      1|         2|            17|
-- |3|      1|         3|            10|
```
```
（重要）
グルーピングにはSELECT内に記載した順番が問われる。
∴ GROPBYの順番を変更したとしても問題はない。
```

### | SUM() - 合計値
e.g.
```SQL
SELECT SUM(quantity) FROM bq_sample.shop_purchases;
```
| |f0_ |
|-|-:  |
|1|2895|

ex.【5.5 演習問題1(1:56)】

shop_id別のquantityの合計値を取得。
quantityの合計の列名を、Total_quantityとする。Total_quantityの降順。
```SQL
SELECT
    shop_id,
    SUM(quantity) AS Total_quantity
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY 1
ORDER BY 2 DESC
;
-- | |shop_id|Taotal_quantity|
-- |1|      1|           1257|
-- |2|      2|           1063|
-- |3|      3|            460|
```
ex.【5.5 演習問題2(3:30)】

shop_id別のsales_amountの合計を取得。
sales_amountの合計の列名をTotal_salesとする。Total_sales多い順。
但、product_idが1,5,11,18に該当するレコードだけを対象。
```SQL
SELECT
    shop_id,
    SUM(sales_amount) AS Total_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id IN(1, 5, 11, 18)
GROUP BY shop_id
ORDER BY Total_sales DESC
;
-- | |shop_id|Total_salse|
-- |1|      1|    1789674|
-- |2|      2|    1422230|
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
|-|-:     |-:       |
|1|      9|    49600|

```SQL
SELECT
    shop_name,
    SUM(quantity) AS ttl_qty,
    SUM(sales) AS ttl_salse
FROM `prj-test3.bq_trial.null_treatment`
GROUP BY 1;
```
| |shop_name|ttl_qty|ttl_salse|
|-|:-       |-:     |-:       |
|1|新宿     |      5|    27800|
|2|渋谷     |      4|    21800|


### | AVG() - 平均値
eg.
```SQL
SELECT AVG(quantity) FROM `prj-test3.bq_sample.shop_purchases`;
```
| |f0_              |
|-|-:               |
|1|2.258190327613107|

eg. ※ `AVG()` の集計時のNULLの振る舞い(計算時は無視される)
```SQL
SELECT
    AVG(quantity) AS avg_qty,
    AVG(sales) AS avg_salse
FROM `prj-test3.bq_trial.null_treatment`;
```
| | avg_qty|avg_salse|
|-|-:      |-:       |
|1|    2.25|  12400.0|

ex.【5.6 演習問題1(2:30)】

shop_id別のquantityの平均を取得。
quantity平均の列名を、avg_atyとして、大きい順に並べ替える。
```SQL
SELECT
    shop_id,
    AVG(quantity) AS avg_qty
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY shop_id
ORDER BY 2 DESC
;
-- | |shop_id|avg_qty           |
-- |1|      5|               3.0|
-- |2|      3|2.4468085106382977|
```

ex.【5.6 演習問題2(3:30)】

shop_id別のsales_amountの平均を取得。
sales_amount平均の列名を、avg_salesとして、大きい順に並べ替える。
但、dateが2018年6月に一致するレコードを対象。
```SQL
SELECT
    shop_id,
    AVG(sales_amount) AS avg_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE
    date BETWEEN "2018-06-01" AND "2018-06-30"
GROUP BY shop_id
ORDER BY 2 DESC
;
-- | |shop_id|avg_sales        |
-- |1|      1|17558.62222222221|
-- |2|      2|12881.13043478261|
```

### | MAX(), MIN() - 最大値、最小値
e.g.
```SQL
SELECT
    MAX(sales_amount) AS max,
    MIN(sales_amount) AS min
FROM `prj-test3.bq_sample.shop_purchases`;
```
| |max  |min |
|-|-:   |-:  |
|1|99000|1400|

ex.【5.7 演習問題1(1:15)】

shop_id毎のproduct_idが15番の商品の1回の販売での最高額を調査。
```SQL
SELECT
    shop_id,
    MAX(sales_amount) AS max_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id = 15
GROUP BY shop_id
ORDER BY 1;

-- | |shop_id|max_sales|
-- |1|      1|    19300|
-- |2|      2|    20000|
```
ex.【5.7 演習問題2(3:15)】

product_idが4番の商品を、一回の販売で1個だけ販売した際に最も低い額で販売した日を、shop_idを調査。
```SQL
SELECT
    date,
    shop_id,
    MIN(sales_amount)AS min_salse
FROM `prj-test3.bq_sample.shop_purchases`
WHERE
    product_id = 4
    AND
    quantity = 1
GROUP BY 1, 2
ORDER BY 3
;
-- | |date      |shop_id|min_salse|
-- |1|2018-06-30|      3|    13260|
-- |2|2018-01-21|      3|    13650|
```

### | 標準偏差（=standard deviation）

    分析対象が母集団（= population）：STDDEV_POP(カラム名)
    分析対象が標本（＝sample）      ：STDDEV_SAMP(カラム名)

ex.【5.8 演習問題1(2:35)】

shop_purchasesテーブルを用いて、店舗毎の１販売あたりの金額のばらつきの大きさをproduct_idが15番の商品について調べる。
指標は、母標準偏差を使用して、std_salesという列名で表示する。ばらつきが小さい順。
```SQL
SELECT
    shop_id,
    STDDEV_POP(sales_amount) AS std_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id = 15
GROUP BY shop_id
ORDER BY 2
;
-- | |shop_id|std_sales         |
-- |1|      4|3664.3411301852543|
-- |2|      1| 4962.301658504852|
```

### | HAVING句 - 集計結果にフィルタリング
e.g.
```SQL
SELECT
    shop_id,
    SUM(quantity) AS sum_qty
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY shop_id
HAVING SUM(quantity) > 1000  --ココ
ORDER BY sum_qty DESC;
```
| |shop_id|sum_qty|
|-|-:     |-:     |
|1|      2|   1257|
|2|      1|   1063|


    ※ WHERE句とHAAVING句の違い
        ・WHERE句は、集計前に実行（絞り込み）される。
        ・HAVING句は、集計後の結果に対して実行される。
       似ているが混同しないように！

    # eg. WHEREで同様な絞り込みをかけると、エラーが返る。
    #      WHERE SUM(quantity) > 1000
    #     (error) Aggregate function SUM not allowed in WHERE clause

ex.【5.9 演習問題1(4:30)】

product_idが18番の商品について、店舗毎の平均売上金額を取得。
また、平均売上金額15,000円を超える店舗だけを表示。
さらに、平均売上金額が大きい順にする。
```SQL
SELECT
    shop_id,
    AVG(sales_amount) AS avg_salse
FROM `prj-test3.bq_sample.shop_purchases`
WHERE product_id = 18
GROUP BY shop_id
HAVING avg_salse > 15000
ORDER BY avg_salse DESC
;
-- | |shop_id|avg_slse          |
-- |1|      4|           25090.0|
-- |1|      2|16971.185185185186|
```
ex.【5.9 演習問題2(10:45)】

customersテーブルに対し、prefecture毎のプレミアムユーザー数（プレミアムユーザーは、Is_premiumの値がtrue）を取得。
プレミアムユーザー数が15人以下のprefectureを、プレミアムユーザー数の多い順に並べ替え。
但、絞り込みにも、並べ替えにも列の別名を利用。
```SQL
SELECT
    prefecture AS pref,
    COUNT(user_id) AS users
FROM `prj-test3.bq_sample.customers`
WHERE
    Is_premium = true
    AND
    prefecture IS NOT NULL
GROUP BY pref
HAVING users <= 15
ORDER BY users DESC
;
-- | |prefecture|users|
-- |1|Osaka     |   11|
-- |2|Kanagawa  |   11|
-- |3|Saitama   |    4|
```

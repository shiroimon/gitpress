---
date   : 2021-09-05
title  : 【BigQuery】分析入門 - Section9
excerpt: Google BigQuery基本の「き」について。
tags   : ["Google BigQuery", "SQL基本", "分析基本"]
---

## || Section9
### | サブクエリ（= 副問い合わせ）
何ができるの？
```
・「全体」と「個別」の対比。（全体の売上を100%とした時、特定の売上は○%を占める。）
・JOINを用いずとも、他のテーブルデータ利用。（JOINをせずにある属性の顧客から売上合計金額データを取得。）
・一度集計したデータの利用。（日別に売上を合算したえ上でそれらの平均を取得できるようになる。）...etc.

※ 実行順序は、「サブクエリ（子クエリ）」>「親クエリ」
```
e.g.
```SQL
# ①
SELECT SUM(quantity) AS ttl_qty
FROM `prj-test3.bq_trial.pos`;
-- | |ttl_qty|
-- |1|     90|

# ②
SELECT
    user_id,
    SUM(quantity) AS qty_by_user
FROM `prj-test3.bq_trial.pos`
GROUP BY  user_id
ORDER BY  2 DESC;
-- | |user_id|qty_by_user|
-- |1|ABC    |         41|
-- |3|XYZ    |         19|
-- |4|WWW    |          7|

# ①+②
SELECT
    user_id,
    SUM(quantity) AS qty_by_user
    -- ①をttl_qtyとして使用可
    (SELECT SUM(quantity) FROM `prj-test3.bq_trial.pos`) AS ttl_qty
FROM `prj-test3.bq_trial.pos`
GROUP BY  user_id
ORDER BY  2 DESC;
-- | |user_id|qty_by_user|ttl_qty|
-- |1|ABC    |         41|     90|
-- |2|STU    |         23|     90|
-- |3|XYZ    |         19|     90|
-- |4|WWW    |          7|     90|
```
ex.【9.2 演習問題1 (6:20)】


```SQL
SELECT
    user_id,
    SUM(quantity) AS qty_by_user,
    (SELECT SUM(quantity) FROM `prj-test3.bq_trial.pos`) AS ttl_aty,
    ROUND(
        SUM(quantity)/(SELECT SUM(quantity) FROM `prj-test3.bq_trial.pos`)
        ,3)*100 AS pctg_by_user
FROM `prj-test3.bq_trial.pos`
GROUP BY 1;

-- | |user_id|qty_by_user|ttl_qty|pctg_by_yser|
-- |1|ABC    |         41|     90|        45.6|
-- |2|STU    |         23|     90|        25.6|
-- |3|XYZ    |         19|     90|         7.8|
-- |4|WWW    |          7|     90|        21.0|
```

### | サブクエリの戻り値の型
```
1. スカラー     : 単一の値    [n]
2. ベクター     : リストの値  [n行×1列]
3. マトリックス : 表の値      [n行×m列]
```
|        |スカラー|ベクター|マトリックス|
|-       |:-:     |:-:     |:-:         |
|SELECT句|   ●   |        |            |
|FROM句  |        |   ●   |     ●     |
|WHERE句 |   ●   |   ●   |            |

#### ■ スカラー
ex.【9.3 演習問題1 (2:10)】(WHERE句 × スカラー)


```SQL
 SELECT
     order_id,
     quantity,
     (SELECT ROUND(AVG(quantity)) FROM `prj-test3.bq_trial.pos`) AS avg_qty
 FROM `prj-test3.bq_trial.pos`
 WHERE quantity >= (SELECT ROUND(AVG(quantity)) FROM `prj-test3.bq_trial.pos`)
 ORDER BY 2 DESC;

-- | |order_id|quantity|avg_aty|
-- |1|       9|      12|    6.0|
-- |2|      12|      12|    6.0|
-- |3|       1|      10|    6.0|
-- |4|       3|       8|    6.0|
```
ex.【9.4 演習問題1 (1:10)】(SELECT句 × スカラー)

```SQL
 SELECT
     order_id, user_id, product_id, quantity,
     (SELECT SUM(quantity) FROM `prj-test3.bq_trial.pos`)AS ttl_qty
 FROM `prj-test3.bq_trial.pos`
 ORDER BY order_id;

-- | |order_id|user_id|product_id|quantity|ttl_qty|
-- |1|       1|ABC    |         1|      10|     90|
-- |2|       2|ABC    |         5|       5|     90|
```
ex.【9.4 演習問題2 (4:20)】(SELECT句 × スカラー)


```SQL
 # (miss_code)
 -- SELECT
 --     user_id,
 --     SUM(unit_price*quantity) AS sales_by_user,
 --     (SELECT SUM(sales_amount) FROM `prj-test3.bq_sample.shop_purchases`) AS ttl_sales,
 --     SUM(unit_price*quantity)/(SELECT SUM(sales_amount) FROM `prj-test3.bq_sample.shop_purchases`) AS sales_share_by_user
 -- FROM `prj-test3.bq_trial.pos`
 -- GROUP BY user_id
 -- ORDER BY 2 DESC;

 # (colect_code)
 SELECT
     user_id,
     SUM(quantity*unit_price) AS sales_by_user,
     (SELECT SUM(quantity*unit_price) FROM `prj-test3.bq_trial.pos`) AS ttl_sales,
     ROUND(
         SUM(quantity*unit_price) / (SELECT SUM(quantity*unit_price) FROM `prj-test3.bq_trial.pos`)
         ,3)*100 AS sales_share_by_user
 FROM `prj-test3.bq_trial.pos`
 GROUP BY user_id
 ORDER BY 2 DESC;

-- | |user_id|sales_by_user|ttl_sales|sales_share_by_user|
-- |1|ABC    |         6020|    14240|               42.3|
-- |2|XYZ    |         3920|    14240|               27.5|
-- |3|STU    |         3080|    14240|               21.6|
```
ex.【9.4 演習問題3 (7:20)】(SELECT句 × スカラー)


```SQL
 SELECT
     DATE_TRUNC(date, month) AS month,
     SUM(sales_amount) AS sales_by_month,
     (SELECT SUM(sales_amount) FROM `prj-test3.bq_sample.shop_purchases`) AS ttl_sales,
     ROUND(
         (SUM(sales_amount)*100) / (SELECT SUM(sales_amount)FROM `prj-test3.bq_sample.shop_purchases`)
         ,1) AS sales_ratio,
 FROM `prj-test3.bq_sample.shop_purchases`
 GROUP BY month
 ORDER BY month;

-- | |month     |sales_by_month|ttl_sales|sales_ratio|
-- |1|2018-01-01|       2056764| 20118392|       10.2|
-- |2|2018-02-01|       1133128| 20118392|        5.6|
-- |3|2018-03-01|       1296473| 20118392|        6.4|
```
ex.【9.5 演習問題1 (0:40)】(WHERE句 × スカラー)


```SQL
 SELECT
     -- *,
     purchase_id,
     sales_amount
 FROM `prj-test3.bq_sample.shop_purchases`
 WHERE
    sales_amount > (SELECT AVG(sales_amount) FROM `prj-test3.bq_sample.shop_purchases`)
 ORDER BY sales_amount DESC;

 -- | |purchase-id|sales_amount|
 -- |1|        995|       99000|
 -- |2|       1203|       99000|
 ```
 ex.【9.5 演習問題2 (3:15)】(WHERE句 × スカラー)


```SQL
 SELECT
     -- *
     user_id,
     CONCAT(last_name, " ", first_name) AS fullname,
     birthday,
     DATE_DIFF("2018-12-31", birthday, year) AS age,
     (SELECT ROUND(AVG(DATE_DIFF("2018-12-31", birthday, year))) FROM `prj-test3.bq_sample.customers`) AS avg_age,
     (SELECT ROUND(STDDEV_POP(DATE_DIFF("2018-12-31", birthday, year))) FROM `prj-test3.bq_sample.customers`)AS stddev_age
 FROM `prj-test3.bq_sample.customers`
 WHERE
    DATE_DIFF("2018-12-31", birthday, year)
    > (SELECT AVG(DATE_DIFF("2018-12-31", birthday, year)) FROM `prj-test3.bq_sample.customers`)
    - (SELECT STDDEV_POP(DATE_DIFF("2018-12-31", birthday, year)) FROM `prj-test3.bq_sample.customers`)
    AND
    DATE_DIFF("2018-12-31", birthday, year)
    < (SELECT AVG(DATE_DIFF("2018-12-31", birthday, year)) FROM `prj-test3.bq_sample.customers`)
    + (SELECT STDDEV_POP(DATE_DIFF("2018-12-31", birthday, year)) FROM `prj-test3.bq_sample.customers`)

    -- age BETWEEN (avg_age + std_age) AND (avg_age - std_age) #(別解)
 ORDER BY age DESC;

 -- |   |user_id|fullname   |birthday   |age|avg_age|stddev_age|
 -- |  1|1105505|松元 陽彦  |1960-05-02| 58|   43.0|      15.0|
 -- |  2|1105505|秋山 充康  |1960-09-26| 58|   43.0|      15.0|
 -- |529| 962102|井ノ口 京香|1989-05-16| 29|   43.0|      15.0|
```
ex.【9.5 演習問題3 (11:10)】(WHERE句 × スカラー)

```SQL
 SELECT
     COUNT(user_id) AS user_in_target,
     (SELECT COUNT(*) FROM `prj-test3.bq_sample.customers`) AS total_user_count,
     ROUND(
         COUNT(user_id)*100/(SELECT COUNT(*) FROM `prj-test3.bq_sample.customers`)
         ,1) AS percentage
 FROM `prj-test3.bq_sample.customers`
 WHERE
    DATE_DIFF("2018-12-31", birthday, year)
    BETWEEN
        (SELECT AVG(DATE_DIFF("2018-12-31", birthday, year)) FROM `prj-test3.bq_sample.customers`)
        -(SELECT STDDEV_POP(DATE_DIFF("2018-12-31", birthday, year)) FROM `prj-test3.bq_sample.customers`)
    AND
        (SELECT AVG(DATE_DIFF("2018-12-31", birthday, year)) FROM `prj-test3.bq_sample.customers`)
        +(SELECT STDDEV_POP(DATE_DIFF("2018-12-31", birthday, year)) FROM `prj-test3.bq_sample.customers`)
 ;

 -- | |user_in_target|total_user_count|percentage|
 -- |1|           529|             944|      56.0|
```
#### ■ ベクター
ex.【9.6 演習問題1 (1:12)】(WHERE句 × ベクター)


```SQL
 #(miss_code)
 -- SELECT
 --     user_id,
 --     SUM(quantity*sales_amount) AS sales_by_users
 -- FROM `prj-test3.bq_sample.shop_purchases`
 -- WHERE (SELECT gender FROM `prj-test3.bq_sample.customers`)=2;

 #(collect_code...34.58 KB)
 SELECT
     user_id,
     SUM(sales_amount) AS sales_by_users
 FROM `prj-test3.bq_sample.shop_purchases`
 WHERE
    user_id IN(SELECT user_id FROM `prj-test3.bq_sample.customers` WHERE gender=2)
 GROUP BY user_id
 ORDER BY user_id;

 -- | |user_id|sales_by_users|
 -- |1| 497520|         31518|
 -- |2| 529565|          3860|

 #(other_code...34.58 KB)
 SELECT
     sp.user_id,
     SUM(sp.sales_amount )AS sales_by_users
 FROM `prj-test3.bq_sample.shop_purchases` AS sp
 INNER JOIN `prj-test3.bq_sample.customers` AS c USING(user_id)
 WHERE c.gender = 2
 GROUP BY 1
 ORDER BY 1;

 -- | |user_id|sales_by_users|
 -- |1| 497520|         31518|
 -- |2| 529565|          3860|
```
ex.【9.6 演習問題2 (4:25)】(WHERE句 × ベクター)


```SQL
 SELECT
     user_id,
     CONCAT(last_name, " ", first_name) AS fullname,
     prefecture,
     birthday
 FROM `prj-test3.bq_sample.customers`
 WHERE
     user_id IN(SELECT user_id FROM `prj-test3.bq_sample.shop_purchases` WHERE product_id =11)
     AND prefecture="Tokyo"
     AND gender=2
 ORDER BY CURRENT_DATE("Asia/Tokyo")-birthday DESC;

 -- | |user_id|fullname    |prefecture|birthday  |
 -- |1|1033150|羽諸 一未  |Tokyo      |1947-08-14|
 -- |2|1089412|巴山 かのこ|Tokyo      |1951-03-31|
```

e.g. 日別の販売数量平均を知りたい場合。等
```SQL
#①:
 SELECT SUM(quantity) AS sum_of_qty_by_day
 FROM `prj-test3.bq_trial.pos`
 GROUP BY date;
 -- | |sum_of_qty_by_day|
 -- |1|               45|
 -- |2|               45|

#②: ①をサブクエリとして利用。
 SELECT
     AVG(sum_of_qty_by_day) AS avg_qty_by_day
 FROM
    (SELECT SUM(quantity) AS sum_of_qty_by_day
     FROM `prj-test3.bq_trial.pos`
     GROUP BY date)
 ;
 -- | |avg_qty_by_day|
 -- |1|          45.0|
```

ex.【9.7 演習問題1 (3:20)】(FROM句 × ベクター)


```SQL
 SELECT
     AVG(sold_by_day) AS avg_number_of__prob_sold_by_day
 FROM
    (SELECT
        COUNT(product_id) AS sold_by_day
     FROM `prj-test3.bq_trial.pos`
     GROUP BY date)
;

 -- | |avg_number_of__prob_sold_by_day|
 -- |1|                            7.5|
```
ex.【9.7 演習問題2 (6:10)】(FROM句 × ベクター)


```sql
 SELECT
     MAX(month_of_pv) AS max_month_of_pv,
     MIN(month_of_pv) AS min_month_of_pv,
     AVG(month_of_pv) AS avg_month_of_pv,
     STDDEV_POP(month_of_pv) AS std_month_of_pv
 FROM
    (SELECT
        DATETIME_TRUNC(timestamp, month) AS month,
        SUM(pageview) AS month_of_pv
    FROM `prj-test3.bq_sample.web_usage`
    GROUP BY 1
    ORDER BY 1)
;
     -- | |month              |month_of_pv|
     -- |1|2018-01-01T00:00:00|        175|
     -- |1|2018-02-01T00:00:00|        233|

 -- | |max_month_of_pv|min_month_of_pv|avg_month_of_pv|std_month_of_pv  |
 -- |1|            320|            118|          216.5|53.40958091329058|
```
#### ■ マトリックス
ex.【9.8 演習問題1 (3:30)】(FROM句 × マトリックス)


```sql
 SELECT
     shop_id,
     ROUND(AVG(day_of_total_amount)) AS avg_sales_amount
 FROM (SELECT
             shop_id,
             date,
             SUM(sales_amount) AS day_of_total_amount
       FROM `prj-test3.bq_sample.shop_purchases`
       GROUP BY 1, 2
       ORDER BY 1, 2)
       --| |shop_id|date      |day_of_total_amount|
       --|1|      1|2018-01-01|              30948|
 GROUP BY 1
 ORDER BY 2 DESC;
 --| |shop_id|avg_sales_amount|
 --|1|      1|         32236.0|
 --|2|      2|         28963.0|
```
ex.【9.8 演習問題2 (5:10)】(FROM句 × マトリックス)

(※ 7.4でerrorとなった演習をサブクエリを用いて改修。）
```sql
 SELECT
     user_id,
     quantity,
     rank
 FROM (SELECT
         order_id,
         user_id,
         quantity,
         RANK() OVER(
             PARTITION BY user_id
             ORDER BY quantity DESC
         ) AS rank
      FROM `prj-test3.bq_trial.pos`
      ORDER BY user_id, rank)
      --| |order_id|user_id|quantity|rank|
      --|1|       9|ABC    |      12|   1|
 WHERE rank <= 3
 ORDER BY 1, 3;
 --| |user_id|quantity|rank|
 --|1|ABC    |      12|   1|
 --|2|ABC    |      10|   2|
 --|3|ABC    |       8|   3|
```
### | サブクエリとJOINの併用
eg.
```SQL
SELECT
     date,
     user_id,
     category,
     quantity
FROM
    (SELECT * FROM `prj-test3.bq_trial.pos` WHERE user_id="ABC") AS pos
INNER JOIN
    `prj-test3.bq_trial.shohin_master` AS sm
ON pos.product_id = sm.product_id
;

-- | |date      |user_id|category|quantity|
-- |1|2019-01-01|ABC    |くだもの|      10|
-- |2|2019-01-01|ABC    |肉      |       5|
```

ex.【9.9 演習問題1 (2:30)】


```SQL
 #(miss_code)
 -- SELECT
 --     sm.category,
 --     SUM(pos.quantity) AS total_quantity
 -- FROM (SELECT * FROM `prj-test3.bq_trial.pos` WHERE quantity >= 10) AS pos
 -- LEFT JOIN `prj-test3.bq_trial.shohin_master`AS sm USING(product_id)
 -- GROUP BY 1
 -- ORDER BY 1 DESC;
 --| |category|total_quantity|
 --|1|魚      |            24|
 --|2|くだもの |            10|

 #(collect_code)
 SELECT
     sm.category AS category,
     SUM(pos.qty_by_prob) AS ttl_qty
 FROM (SELECT
         product_id,
         SUM(quantity) AS qty_by_prob
       FROM `prj-test3.bq_trial.pos`
       GROUP BY 1
       HAVING SUM(quantity) > 10) AS pos
       --| |product_id|qty_by_prob|
       --|1|         5|         13|
 LEFT JOIN `prj-test3.bq_trial.shohin_master`AS sm USING(product_id)
 GROUP BY sm.category;
 --| |category|ttl_qty|
 --|1|肉      |     13|
 --|2|野菜    |     30|
 --|3|魚      |     24|
```
ex.【9.9 演習問題2 (6:50)】


```sql
 #(miss_code)
 -- SELECT
 --   c.prefecture
 --   RANK() OVER(
 --     PARTITION BY c.prefecture
 --     ORDER BY sp_ttl.total_amount
 --   ) AS rank
 -- FROM (SELECT *
 --       FROM `prj-test3.bq_sample.customers`
 --       WHERE prefecture IS NOT NULL) AS c
 -- LEFT JOIN (SELECT user_id, SUM(sales_amount) AS total_amount
 --           FROM `prj-test3.bq_sample.shop_purchases`
 --           GROUP BY 1) AS sp_ttl ON c.user_id=sp_ttl.user_id
 -- LEFT JOIN `prj-test3.bq_sample.shop_purchases` AS sp ON c.user_id=sp.user_id

 #(collect_code)
 SELECT *
 FROM (SELECT
           cu.prefecture AS pref,
           sp.purchase_id,
           sp.sales_amount,
           RANK() OVER(
                 PARTITION BY prefecture
                 ORDER BY sp.sales_amount DESC
           ) AS rank
       FROM `prj-test3.bq_sample.shop_purchases` AS sp
       LEFT JOIN `prj-test3.bq_sample.customers` AS cu
       ON sp.user_id=cu.user_id)
       --| |pref      |purches_id|sales_amount|rank|
       --|1|Saitama   |       178|        1930|   1|
 WHERE pref IS NOT NULL AND rank <= 3
 ORDER BY pref, rank;
 --| |pref      |purches_id|sales_amount|rank|
 --|1|Aichi     |       193|       75270|   1|
 --|2|Aichi     |       247|       43800|   2|
 --|3|Aichi     |       535|       43800|   2|
 --|1|Aomori    |       206|       22000|   1|
```
ex.【9.9 演習問題 (11:10)】


```sql
--|pref |prodcut_id|ttl_sales_amount|rank|
--|Tokyo|       120|         5000000|   1|

SELECT *
FROM (SELECT
           cu.prefecture AS pref,
           sp.product_id,
           SUM(sp.sales_amount) AS total_sales,
           RANK() OVER(
               PARTITION BY cu.prefecture
               ORDER BY SUM(sp.sales_amount) DESC
           ) AS rank
       FROM `prj-test3.bq_sample.shop_purchases` AS sp
       LEFT JOIN `prj-test3.bq_sample.customers` AS cu USING(user_id)
       GROUP BY 1, 2
       ORDER BY 1
       )
WHERE pref IS NOT NULL AND rank<=3
ORDER BY pref, rank;

--| |pref  |prodcut_id|total_sales|rank|
--|1|Aichi |        9|      111360|   1|
--|2|Aichi |       10|      102200|   2|
--|3|Aichi |        4|       90324|   3|
--|1|Aomori|        6|       22000|   1|


SELECT *
FROM (SELECT
           prefecture,
           product_id,
           sales,
           RANK() OVER(
               PARTITION BY prefecture
               ORDER BY sales DESC
           ) AS sales_rank
       FROM (SELECT
                 cu.prefecture,
                 sp.product_id,
                 SUM(sp.sales_amount) AS sales
             FROM `prj-test3.bq_sample.shop_purchases`AS sp
             LEFT JOIN `prj-test3.bq_sample.customers` AS cu ON sp.user_id = cu.user_id
             GROUP BY cu.prefecture, sp.product_id
            )
      )
WHERE prefecture IS NOT NULL AND sales_rank <=3
ORDER BY 1, 4;

--| |pref  |prodcut_id|total_sales|rank|
--|1|Aichi |        9|      111360|   1|
--|2|Aichi |       10|      102200|   2|
--|3|Aichi |        4|       90324|   3|
```

### | WITH句 - サブクエリの可読性を高める
```
※書式
    WITH
    [後から参照する名前1] AS (SELECT句から始まるサブクエリ1),
    [後から参照する名前2] AS (SELECT句から始まるサブクエリ2)

あたかも予め用意された様なテーブルデータとして
結合させたり、利用できたりする。
```
e.g.
```SQL
WITH agg AS (
    SELECT
        DATE_TRUNC(date, month) AS month,
        shop_id,
        SUM(sales_amount) AS total_sales
    FROM `prj-test3.bq_sample.shop_purchases`
    GROUP BY 1, 2)
    --| |month     |shop_id|total_sales|

SELECT
     agg.month,
     sm.chief_name,
     SUM(agg.total_sales) AS sales
FROM `prj-test3.bq_sample.shops_master` AS sm
INNER JOIN agg ON agg.shop_id = sm.shop_id
GROUP BY 1, 2
ORDER BY 1, 3;

-- | |month     |chief_name   |sales |
-- |1|2018-01-01|大井谷 みすず|435091|
-- |2|2018-01-01|山下  唐三郎 |723369|
```

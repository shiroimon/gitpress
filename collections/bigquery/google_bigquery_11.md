---
date    : 2021-09-03
title   : 🔍 分析入門 Section11: 練習問題
excerpt : ---
tags    : ["Google BigQuery", "SQL", "分析", "Udemy講座"]
---


![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)


## | Section11
### | 難易度：低

■ practice 11.2 (難易度:低)

平成生まれ（平成は1989年1月8日〜2019年4月30日まで）の男性で最も人気のファーストネームはなんですか？
```SQL
#(goal_image)
--| |first_name|count|rank|

SELECT
    first_name,
    COUNT(*) AS count,
    RANK () OVER(ORDER BY COUNT(*) DESC) AS rank
FROM `prj-test3.bq_sample.customers`
WHERE
    gender=1 --男性
    AND
    birthday BETWEEN "1989-01-08" AND "2019-04-30" -- 平成産まれ
GROUP BY 1
ORDER BY 2 DESC;

--| |first_name|count|rank|
--|1|匠        |    2|   1|
-- (1.6s 22KB)
```

■ practice 11.3 (難易度:低)

都道府県別の平均年齢が最も高い都道府県は？
customersテーブルを利用して、都道府県と平均年齢（小数点以下を四捨五入して）を整数で回答。
但、5人以上顧客がいるえ都道府県のみを対象。年齢は2018年12月31日時点での満年齢で計算。
```SQL
#(goal_image)
--| |pref|avg_age|

SELECT
    prefecture AS pref,
    ROUND(
        AVG(
            -- CAST(FORMAT_DATE("%Y" ,DATE_TRUNC(CURRENT_DATE("Asia/Tokyo"), MONTH)) AS INT64)
            CAST(FORMAT_DATE("%Y" ,DATE_TRUNC("2018-12-31", MONTH)) AS INT64)
          - CAST(FORMAT_DATE("%Y", (DATE_TRUNC(birthday, MONTH))) AS INT64)
          )
        ) AS avg_age
FROM `prj-test3.bq_sample.customers`
WHERE prefecture IS NOT NULL
GROUP BY 1
HAVING COUNT(prefecture) > 5
ORDER BY 2 DESC;

-- | |pref     |avg_age|
-- |1|Miyagi|   51.0|
-- (0.5s 13.9KB)

# (other-code)
# SELECT
#    prefecture,
#    ROUND(AVG(DATE_DIFF("2018-12-31", birthday, YEAR))) AS avg_age,
#    COUNT(user_id) AS users
```

■ practice 11.4 (難易度:低)
```SQL
#(goal_image)
--| |prob_category|total_qty|
--|1|Tシャツ      |         |
--|2|Non-Tシャツ  |         |

SELECT
    prob_category,
    SUM(qty) AS total_qty
FROM(SELECT
          -- CASE
          --     WHEN REGEXP_CONTAINS(pm.prod_name, r"%Tシャツ%") THEN "Tシャツ"
          --     ELSE "Non-Tシャツ"
          -- END AS prob_category,
          IF(pm.product_id>=11, "Tシャツ", "Non-Tシャツ") AS prob_category,
          sp.quantity AS qty
    FROM `prj-test3.bq_sample.shop_purchases` AS sp
    LEFT OUTER JOIN `prj-test3.bq_sample.products_master` AS pm USING(product_id))
GROUP BY 1
ORDER BY 2 DESC;

--| |prob_category|total_qty|
--|1|Tシャツ      |     1739|
--|2|Non-Tシャツ  |     1156|
--(0.8s 20.2KB)

-- answer -----------------------------------------------------
SELECT
    CASE prod_name LIKE "%Tシャツ%"
        WHEN TRUE THEN "Tシャツ"
        ELSE "Non-Tシャツ"
    END AS t_shirts_or_not,
    SUM(quantity) AS ttl_qty
FROM `prj-test3.bq_sample.shop_purchases`
LEFT JOIN `prj-test3.bq_sample.products_master`
USING(product_id)
GROUP BY t_shirts_or_not;

--| |prob_category|total_qty|
--|1|Non-Tシャツ  |     1156|
--|2|Tシャツ      |     1739|
--(0.5s 20.8KB)
```

■ practice 11.5 (難易度:低)
```SQL
SELECT
    COUNT(user_id)
FROM
    (SELECT
        user_id
     FROM
        (SELECT * FROM `prj-test3.bq_sample.shop_purchases` WHERE date BETWEEN "2018-01-01" AND "2018-01-31")
     INTERSECT DISTINCT
     SELECT
        user_id
     FROM
        (SELECT * FROM `prj-test3.bq_sample.shop_purchases` WHERE date BETWEEN "2018-02-01" AND "2018-02-28")
    )
;
-- | |f0_|
-- |1|  4|
-- (1.0s 20KB)


-- answer -----------------------------------------------------
SELECT COUNT(user_id) AS users
FROM(
    (SELECT user_id FROM `prj-test3.bq_sample.shop_purchases` WHERE DATE_TRUNC(date, MONTH)="2018-01-01")
    INTERSECT DISTINCT
    (SELECT user_id FROM `prj-test3.bq_sample.shop_purchases` WHERE DATE_TRUNC(date, MONTH)="2018-02-01")
);

-- | |users|
-- |1|    4|
-- (0.8s 20KB)
```

■ practice 11.6 (難易度:低)
```SQL
-- ||term |min_sales_amount|max_sales_amount|diff_sales_amount|
-- ||term1|
-- ||term2|
-- ||term3|
#(miss_code)
SELECT *
FROM((SELECT
            DATE_TRUNC(date, MONTH) AS month,
            MIN(sales_amount) AS min_sales_amount,
            MAX(sales_amount) AS max_sales_amount,
            MAX(sales_amount)-MIN(sales_amount) AS diff_sales_amount,
            COUNT(purchase_id) AS count_purchase
      FROM `prj-test3.bq_sample.shop_purchases`
      WHERE DATE_TRUNC(date, MONTH) IN ("2018-01-01", "2018-02-01", "2018-03-01", "2018-04-01")
      GROUP BY 1)
      UNION ALL
      (SELECT
            DATE_TRUNC(date, MONTH) AS month,
            MIN(sales_amount) AS min_sales_amount,
            MAX(sales_amount) AS max_sales_amount,
            MAX(sales_amount)-MIN(sales_amount) AS diff_sales_amount,
            COUNT(purchase_id) AS count_purchase
       FROM `prj-test3.bq_sample.shop_purchases`
       WHERE DATE_TRUNC(date, MONTH) IN ("2018-05-01", "2018-06-01", "2018-07-01", "2018-08-01")
       GROUP BY 1)
       UNION ALL
      (SELECT
            DATE_TRUNC(date, MONTH) AS month,
            MIN(sales_amount) AS min_sales_amount,
            MAX(sales_amount) AS max_sales_amount,
            MAX(sales_amount)-MIN(sales_amount) AS diff_sales_amount,
            COUNT(purchase_id) AS count_purchase
       FROM `prj-test3.bq_sample.shop_purchases`
       WHERE DATE_TRUNC(date, MONTH) IN ("2018-09-01", "2018-10-01", "2018-11-01", "2018-12-01")
       GROUP BY 1)
) ORDER BY month;


-- answer -----------------------------------------------------
SELECT
    DATE_TRUNC(date, QUARTER) AS quarter,
    MIN(sales_amount) AS min_sales,
    MAX(sales_amount) AS max_sales,
    MAX(sales_amount)-MIN(sales_amount) AS diff
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY 1
ORDER BY 4 DESC;

-- | |quarter   |min_sales|max_sales|diff |
-- |1|2018-10-01|     1400|    99000|97600|
-- |2|2018-07-01|     1600|    90710|89110|
-- (0.7s 20KB)
```
### | 難易度：中

■ practice 11.7(難易度:中)
```SQL
--||階級 |度数（階級の個数）|相対度数（構成比）|
--||class|frequency         |relative_frequency|

SELECT
    class,
    COUNT(class) AS frequency,
    ROUND(COUNT(class)/1282, 3) AS relative_frequency
FROM(SELECT
        CASE
            WHEN sales_amount < 10000 THEN "class_1"
            WHEN sales_amount < 20000 THEN "class_2"
            WHEN sales_amount < 30000 THEN "class_3"
            WHEN sales_amount < 40000 THEN "class_4"
            WHEN sales_amount < 50000 THEN "class_5"
            WHEN sales_amount < 60000 THEN "class_6"
            WHEN sales_amount < 70000 THEN "class_7"
            WHEN sales_amount < 80000 THEN "class_8"
            WHEN sales_amount < 90000 THEN "class_9"
            WHEN sales_amount < 100000 THEN "class_10"
            ELSE "other"
        END AS class
     FROM `prj-test3.bq_sample.shop_purchases`
)
GROUP BY
     1
ORDER BY
     3 DESC
;

-- | |class   |frequency|relative_frequency|
-- |1|class_1 |      590|              0.46|
-- |2|class_2 |      388|             0.303|
-- |3|class_3 |      130|             0.101|
-- |8|class_10|        7|             0.005|
-- (0.4s 10KB)


-- answer -----------------------------------------------------
SELECT
    kaikyu,
    dosuu,
    ROUND(dosuu/(SELECT COUNT(*) FROM `prj-test3.bq_sample.shop_purchases`), 3) AS soutai_dosuu
FROM(SELECT
        CASE
            WHEN sales_amount < 10000 THEN "0-10000"
            WHEN sales_amount < 20000 THEN "10000-20000"
            WHEN sales_amount < 30000 THEN "20000-30000"
            WHEN sales_amount < 40000 THEN "30000-40000"
            WHEN sales_amount < 50000 THEN "40000-50000"
            WHEN sales_amount < 60000 THEN "50000-60000"
            WHEN sales_amount < 70000 THEN "60000-70000"
            WHEN sales_amount < 80000 THEN "70000-80000"
            WHEN sales_amount < 90000 THEN "80000-90000"
            WHEN sales_amount < 100000 THEN "90000-100000"
            ELSE "100000-"
        END AS kaikyu,
     COUNT(*) AS dosuu
     FROM `prj-test3.bq_sample.shop_purchases`
     GROUP BY kaikyu)
ORDER BY 1
;
-- | |kaikyu      |dosuu|soutai_dossu|
-- |1|0-10000     |  590|        0.46|
-- |2|10000-20000 |  388|       0.303|
-- (1.1s 10KB)
```

■ practice 11.8(難易度:中)
```SQL
#(miss_codd)
-- SELECT
--     prod_name,
--     avg_sales_amount - list_price AS diff
-- FROM(SELECT
--          product_id,
--          ROUND(AVG(sales_amount), 2) AS avg_sales_amount
--      FROM `prj-test3.bq_sample.shop_purchases`
--      GROUP BY 1
--      ORDER BY 1) AS avg_prod
-- LEFT OUTER JOIN `prj-test3.bq_sample.products_master` AS pm USING(product_id);
-- | |prod_name                      |diff    |
-- |1|オックスフォードシャツ 綿 100% |22885.28|
-- |2|オックスフォードシャツ 麻混    |19281.67|


-- answer -----------------------------------------------------
SELECT
    pm.prod_name,
    ROUND(ttl_amouont/ttl_qty, 2) AS aov,
    list_price,
    ROUND((list_price - (ttl_amouont/ttl_qty))*100 / list_price, 3) AS discount_rate
FROM(SELECT
        product_id,
        SUM(quantity) AS ttl_qty,
        SUM(sales_amount) AS ttl_amouont
     FROM `prj-test3.bq_sample.shop_purchases`
     GROUP BY product_id
     --| |product_id|ttl_qty|ttl_amouont|
     --|1|         1|     82|    1536670|
     --|2|         2|     76|    1370940|
) AS sp
JOIN `prj-test3.bq_sample.products_master` AS pm
USING(product_id)
ORDER BY 4 DESC
;
-- | |prod_name                            |aov   |list_price|discount_rate|
-- |1|Tシャツ（キャラクター・子供用） 長袖|1848.4|      2000|         7.58|
-- |2|ポロシャツ 長袖                     |8204.5|      8800|        6.767|
```

■ practice 11.10(難易度:中)
```SQL
SELECT
    CASE
        WHEN cu.age <= 20 THEN "20歳以下"
        WHEN cu.age <= 40 THEN "21歳~40歳"
        WHEN cu.age <= 60 THEN "41歳~60歳"
        WHEN cu.age <= 80 THEN "61歳~80歳"
        ELSE "81歳以上"
    END AS age_category,
    SUM(sp.sales_amount) AS total_amount
FROM `prj-test3.bq_sample.shop_purchases` AS sp
LEFT OUTER JOIN
    (SELECT
         user_id,
         DATE_DIFF("2018-12-31", birthday, YEAR) AS age
     FROM `prj-test3.bq_sample.customers`) AS cu
USING(user_id)
GROUP BY 1
ORDER BY 2
;
-- | |age_category|total_amount|
-- |1|81歳以上    |      441141|
-- |2|20歳以下    |      770006|
-- (0.9s 34.6KB)

-- answer -----------------------------------------------------
SELECT
    cu.age_group,
    SUM(sp.sales_amount) AS sales
FROM(SELECT
        user_id,
        CASE
            WHEN DATE_DIFF("2018-12-31", birthday, YEAR) <= 20 THEN "20歳以下"
            WHEN DATE_DIFF("2018-12-31", birthday, YEAR) <= 40 THEN "21歳~40歳"
            WHEN DATE_DIFF("2018-12-31", birthday, YEAR) <= 60 THEN "41歳~60歳"
            WHEN DATE_DIFF("2018-12-31", birthday, YEAR) <= 80 THEN "61歳~80歳"
            WHEN DATE_DIFF("2018-12-31", birthday, YEAR) > 80 THEN "81歳以上"
            ELSE "年齢不明"
        END AS age_group
    FROM `prj-test3.bq_sample.customers`) AS cu
RIGHT JOIN `prj-test3.bq_sample.shop_purchases` AS sp
USING(user_id)
GROUP BY 1
ORDER BY 2 DESC;
-- | |age_group|sales  |
-- |1|21歳~40歳|8985952|
-- |2|41歳~60歳|6321162|
-- |4|20歳以下 | 770006|
-- (1.0s 34.6KB)
```

■ practice 11.11(難易度:中)
```SQL
SELECT product_id
FROM `prj-test3.bq_sample.shop_purchases`
WHERE shop_id = 2 AND DATE_TRUNC(date, MONTH) = "2018-02-01"
GROUP BY 1
EXCEPT DISTINCT
SELECT product_id
FROM `prj-test3.bq_sample.shop_purchases`
WHERE shop_id = 2 AND DATE_TRUNC(date, MONTH) = "2018-01-01"
GROUP BY 1;

-- | |product_id|
-- |1|         8|
-- (0.9s 30KB)

-- answer -----------------------------------------------------
WITH jan_sales AS (
         SELECT shop_id, product_id
         FROM `prj-test3.bq_sample.shop_purchases`
         WHERE DATE_TRUNC(date, MONTH)="2018-01-01" AND shop_id=2
         GROUP BY 1, 2),
     feb_sales AS (
         SELECT shop_id, product_id
         FROM `prj-test3.bq_sample.shop_purchases`
         WHERE DATE_TRUNC(date, MONTH)="2018-02-01" AND shop_id=2
         GROUP BY 1, 2)

SELECT shop_id, product_id FROM feb_sales
EXCEPT DISTINCT
SELECT shop_id, product_id FROM jan_sales;

-- | |shop_id|product_id|
-- |1|      2|         8|
-- (1.1s 30KB)


```
### | 難易度：高

■ practice 11.12(難易度:高)
```SQL
-- |prod_id|prod_name|first_by_sel_qty|rank|

SELECT
    product_id,
    prod_name,
    COUNT(product_id)
FROM(SELECT
        *,
        RANK() OVER(
            PARTITION BY user_id --購入者別
            ORDER BY purchase_id --購入ID昇順
        ) AS ranking
     FROM `prj-test3.bq_sample.shop_purchases`
     ORDER BY user_id) AS rk
     -- ||||user_id|||rank|
     -- |||| 733995|||   1|
LEFT OUTER JOIN `prj-test3.bq_sample.products_master` AS pm
USING(product_id)
WHERE ranking = 1
GROUP BY 1,2
ORDER BY 3 DESC
LIMIT 3;

-- | |product_id|prod_name                    |f0_|
-- |1|        10|ブラウス 長袖                | 69|
-- |2|        11|Tシャツ（デザイン） 半袖     | 66|
-- |3|        13|Tシャツ（キャラクター） 半袖 | 65|
-- (1.2s 30.9KB)

-- answer -----------------------------------------------------
SELECT
    pm.prod_name AS first_purchase_item_name,
    COUNT(*) AS number_of_users
FROM(SELECT
         user_id,
         MAX(first_purchase_item) AS first_purchase_id
     FROM(SELECT
             user_id,
             first_value(product_id) OVER(
                 PARTITION BY user_id
                 ORDER BY purchase_id
             ) AS first_purchase_item
         FROM `prj-test3.bq_sample.shop_purchases`)
            -- | |user_id|first_purchase_item|
            -- |1| 496070|                  2|
     GROUP BY user_id) AS fp
        -- | |user_id|first_purchase_id|
        -- |1| 496070|                2|
JOIN `prj-test3.bq_sample.products_master` AS pm
ON fp.first_purchase_id = pm.product_id
GROUP BY first_purchase_item_name
ORDER BY 2 DESC
LIMIT 3;

-- | |first_purchase_item_name      |number_of_users|
-- |1|ブラウス 長袖                 |             69|
-- |2|Tシャツ（デザイン） 半袖      |             66|
-- |3|Tシャツ（キャラクター） 半袖  |             65|
-- (1.2s 30.9KB)
```

■ practice 11.13(難易度:高)
```SQL
-- ||product_id|count_item|

-- SELECT
--     product_id,
--     COUNT() AS count_item
-- FROM
-- ;

-- id20を買った人リスト
-- SELECT user_id FROM `prj-test3.bq_sample.shop_purchases` WHERE product_id=20;

-- id20しか買ってない人リスト
-- SELECT user_id
-- FROM (SELECT *, RANK() OVER( PARTITION BY user_id ORDER BY product_id) AS ranking FROM `prj-test3.bq_sample.shop_purchases`)
-- WHERE
-- user_id IN (SELECT user_id FROM `prj-test3.bq_sample.shop_purchases` WHERE product_id=20)
-- AND
-- rank=1 AND product_id=20

-- ランキングテーブル
-- SELECT *, RANK() OVER( PARTITION BY user_id ORDER BY product_id) AS ranking FROM `prj-test3.bq_sample.shop_purchases`;

-- id20 かっ複数買った人テーブル
-- SELECT *
-- FROM(SELECT *, RANK() OVER( PARTITION BY user_id ORDER BY product_id) AS ranking FROM `prj-test3.bq_sample.shop_purchases`)
-- WHERE user_id IN (SELECT user_id FROM `prj-test3.bq_sample.shop_purchases` WHERE product_id=20);


WITH
    ranking AS (
        SELECT
            *,
            RANK() OVER(
                PARTITION BY user_id --購入者別
                ORDER BY product_id ASC --商品ID昇順∵
                ) AS rank
        FROM `prj-test3.bq_sample.shop_purchases`)

SELECT
    pm.prod_name,
    market_basket.count_prod
FROM(SELECT
        product_id,
        COUNT(product_id) AS count_prod
     FROM ranking
     WHERE
        -- id20 買った人リスト
        user_id IN (SELECT user_id FROM `prj-test3.bq_sample.shop_purchases` WHERE product_id=20)
        AND
        -- id20 しか買ってない人リスト
        user_id NOT IN (SELECT user_id
                        FROM ranking
                        WHERE
                            user_id IN (SELECT user_id FROM `prj-test3.bq_sample.shop_purchases` WHERE product_id=20)
                            AND
                            rank=1
                            AND product_id=20)
        AND
        product_id != 20
     GROUP BY 1) AS market_basket
LEFT JOIN `prj-test3.bq_sample.products_master` AS pm USING(product_id)
ORDER BY 2 DESC;

-- | |product_id|prod_name              |count_prod|
-- |1|        17|Tシャツ（コラボ） 半袖 |         7|
-- |2|         8|ポロシャツ 長袖        |         6|
-- (2.6s 20.8KB)


-- answer -----------------------------------------------------

WITH purchase_master AS (
    SELECT user_id, prodcut_id, COUNT(*) AS kaisu
    FROM `prj-test3.bq_sample.shop_purchases`
    GROUP BY 1, 2
)
SELECT
    prod1,
    prod2,
    COUNT(*) AS kaisu
FROM(SELECT
         pos1.product_id AS prod1,
         pos2.product_id AS prod2
     FROM purchase_master AS pos1
     JOIN purchase_master AS pos2
     ON pos1.user_id = pos2.user_id AND pos1.product_id != pos2.product_id
      --【 SELF JOIN 】
      -- | |prod1|prod2|
      -- |1|    1|    2|
      -- |2|    1|    2|
      -- |3|    1|    2|
      -- |4|    1|    3|
    )
WHERE prod1 = 20
GROUP BY prod1, prod2
ORDER BY 3 DESC;

-- | |prod1|prod2|kaisu|
-- |1|   20|   17|    7|
-- |2|   20|    8|    6|
-- (0.6s 20KB)
```

■ practice 11.14(難易度:高)
```SQL
#(miss_code)
-- WITH
-- -- cv uu
-- cv_uu AS(
--     SELECT
--         DISTINCT user_id
--     FROM `prj-test3.bq_sample.shop_purchases`),
-- -- logging
-- logging AS (
--     SELECT
--         *,
--         RANK() OVER(
--             PARTITION BY user_id
--             ORDER BY timestamp ASC
--             ) AS user_logging
--     FROM `prj-test3.bq_sample.web_usage`)

-- SELECT
-- user_id,
-- COUNT(user_id) AS uu
-- FROM(SELECT
--             *,
--             REGEXP_CONTAINS(lg.page, r"\?") AS pd
--      FROM cv_uu AS cv
--      LEFT OUTER JOIN logging AS lg USING(user_id)
--      WHERE lg.session_count IS NOT NULL)
-- WHERE pd IS TRUE
-- GROUP BY 1;

-- answer -----------------------------------------------------
SELECT
    COUNT(DISTINCT user_id) AS target_user
FROM(
    SELECT
            sp.user_id,
            sp.first_date,
            sp.first_product_id,
            DATE(DATETIME_TRUNC(wu.timestamp, DAY)) AS hit_date,
            REGEXP_EXTRACT(wu.page, r"\d+$") AS viewd_product_id
    FROM(
        -- ユーザー別、初回購入商品ID一覧（重複除去）
        SELECT
                user_id,
                MIN(date) AS first_date,
                MIN(first_product_id) AS first_product_id
        FROM(
            -- ユーザー別、初回購入商品ID一覧
            SELECT
                    user_id,
                    date,
                    FIRST_VALUE(product_id) OVER(
                            PARTITION BY user_id
                            ORDER BY purchase_id
                    ) AS first_product_id
            FROM `prj-test3.bq_sample.shop_purchases`
            --| |user_id|date      |first_product_id|
            --|1| 496070|2018-12-29|               2|
        )
        GROUP BY user_id
        --| |user_id|first_date |first_product_id|
        --|1| 496070|2018-12-29|               2|
    ) AS sp
    JOIN `prj-test3.bq_sample.web_usage` AS wu USING(user_id)
    WHERE
            DATE(DATETIME_TRUNC(wu.timestamp, DAY)) < sp.first_date
            AND
            REGEXP_EXTRACT(wu.page, r"\d+$") = CAST(sp.first_product_id AS STRING)
    --| |user_id|first_date|first_product_id|hit_date  |viewd_product_id|
    --|1| 597427|2018-10-20|              17|2018-01-20|17             |
);

--| |target_user|
--|1|        20|
```

■ practice 11.15(難易度:高)
```SQL
--<A>
-- SELECT
--     medium,
--     COUNT(session_count) AS ss
-- FROM `prj-test3.bq_sample.web_usage`
-- GROUP BY medium
-- ORDER BY ss DESC;

--<B>
-- SELECT
--     user_id,
--     medium,
--     MIN(first_landing) AS first_landing,
--     MAX(landing_exit) AS landing_exit,
--     MIN(ss_time) AS ss_time
-- FROM(
    -- SELECT
    --     user_id,
    --     medium,
    --     yyyy_mm_dd ,
    --     FIRST_VALUE(hh_mm_ss) OVER(
    --         PARTITION BY user_id
    --         ORDER BY timestamp ASC
    --         ) AS first_landing_time,
    --     LAST_VALUE(hh_mm_ss) OVER(
    --         PARTITION BY user_id
    --         ORDER BY timestamp ASC
    --         ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    --         ) AS landing_exit_time,
    --
    --     -- AS ss_time
    -- FROM(
    --     SELECT
    --         user_id,
    --         medium,
    --         timestamp,
    --         DATE(DATETIME_TRUNC(timestamp, DAY)) AS yyyy_mm_dd,
    --         TIME(DATETIME_TRUNC(timestamp, SECOND)) AS hh_mm_ss
    --     FROM  `prj-test3.bq_sample.web_usage`
    -- );
-- )
-- GROUP BY 1, 2;


-- answer -----------------------------------------------------
SELECT
    medium,
    SUM(session) AS session,
    SUM(session_duration) AS session_duration,
    ROUND((SUM(session_duration)/SUM(session))/60, 2) AS avg_session_duration_minute
FROM(
    SELECT
        user_id,
        session_count,
        medium,
        COUNT(DISTINCT CONCAT(CAST(user_id AS STRING), "_", CAST(session_count AS STRING))) AS session,
        MIN(session_duration) AS session_duration
    FROM(
        SELECT
            user_id,
            session_count,
            medium,
            FIRST_VALUE(timestamp) OVER(PARTITION BY user_id, session_count ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING ) AS session_start_time,
            LAST_VALUE(timestamp) OVER(PARTITION BY user_id, session_count ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING ) AS session_end_time,
            DATETIME_DIFF(
                LAST_VALUE(timestamp) OVER(PARTITION BY user_id, session_count ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING ),
                FIRST_VALUE(timestamp) OVER(PARTITION BY user_id, session_count ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING ),
                SECOND) AS session_duration
        FROM `prj-test3.bq_sample.web_usage`
        GROUP BY user_id, session_count, medium, timestamp
        --| |user_id|session_count|medium  |session_start_time |session_end_time   |session_duration|
        --|1| 855650|            8|referral|2018-08-08T18:48:48|2018-08-08T18:48:48|               0|
        --|2| 916550|            1|organic |2018-10-22T00:46:50|2018-10-22T00:47:31|              41|
    )
    GROUP BY user_id ,session_count, medium
    -- | |user_id|session_count|medium |session|session_duration|
    -- |1| 989072|            1|organic|      1|             289|
    -- |2| 995450|            4|organic|      1|               0|
    -- |3|1064305|            3|(none) |      1|            2179|
)
GROUP BY medium
ORDER BY 4 DESC;

-- | |medium  |session|session_duration|avg_session_duration_minute|
-- |1|social  |     15|            3262|                       3.62|
-- |2|organic |    556|          112432|                       3.37|
-- |3|referral|     84|           13741|                       2.73|
```
Cf.[日付と時刻を取得する(date関数, time関数, datetime関数, julianday関数, strftime関数)](https://www.dbonline.jp/sqlite/function/index6.html)


■ practice 11.16(難易度:高)
```SQL
WITH transaction_user AS(
    --ネット購入者リスト
    SELECT
        user_id,
        MIN(transaction_product) AS transaction_product
    FROM(
        SELECT
            user_id,
            transaction_product
        FROM `prj-test3.bq_sample.web_usage`
        WHERE transaction_id IS NOT NULL
    )
    GROUP BY user_id
)

SELECT
    user_id,
    last_name,
    first_name,
    shop_name,
    product_id,
    prod_name
FROM(
    SELECT
        *
    FROM(
        SELECT
            *
        FROM(
            --リアル店舗購入者
            SELECT
                *
            FROM `prj-test3.bq_sample.shop_purchases` AS sp
            LEFT JOIN transaction_user AS cv USING(user_id)
            WHERE
                cv.transaction_product IS NOT NULL
                AND
                sp.product_id = cv.transaction_product
        )
        LEFT JOIN `prj-test3.bq_sample.shops_master` USING(shop_id)
    )
    LEFT JOIN `prj-test3.bq_sample.customers` USING(user_id)
)
LEFT JOIN `prj-test3.bq_sample.products_master` USING(product_id);

-- | |user_id|last_name|first_name|shop_name   |product_id|prod_name          |
-- |1|1000380|浅井      |忠        |自由が丘店|         3|開襟シャツ 綿 100%  |
-- |2|1011670|度會      |咲耶      |下北沢店  |        16|Tシャツ（漢字） 長袖|
-- (0.9s 78.7KB)

-- answer -----------------------------------------------------
```

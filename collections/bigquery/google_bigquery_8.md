---
date    : 2021-09-06
title   : 🔍 分析入門 
excerpt : - Section8: テーブル結合
tags    : ["Google BigQuery", "SQL", "分析", "ER図", "join", "Udemy講座"]
---

## || Section8
### | ER図

    ER図（Entity-Relations）: テーブル同士の関係性を表した図表

![img](https://i.gyazo.com/14de2286b6d4ee7163677735cc745f61.png)

```
そもそも、DBが複数のテーブルを持っているのは?
∵ カスタマイズ性、メンテナンス性を高めることが重視されているため。
```

### | キーの種類
```
🔑 主キー(PK:PrimaryKey)   :
🗝 外部キー(FK:ForeignKey) : 外部テーブルと結合するためのキー
```

### | JOIN - 結合
※ JOINの種類
```SQL
1. INNER JOIN       : 左右両方のテーブルに[FK=PK]が存在するレコードだけを結合（いずれにも該当しないレコードは弾かれる）
2. LEFT OUTER JOIN  : 左右にあろうが、無かろうが、兎に角「左を優先して！」結合
3. RIGHT OUTER JOIN : 左右にあろうが、無かろうが、兎に角「右を優先して！」結合
4. FULL OUTER JOIN  : 左右にあるだけのレコードを「全部まとめて出してくれ！」結合
5. CROSS JOIN       :
```
e.g. 書式
```SQL
SELECT
FROM テーブルA AS A
(INNER|LEFT[OUTER]|RIGHT[OUTER]|FULL[OUTER])JOIN テーブルB AS B
→ USING(FK-PKの両テーブルに共通するカラム)
→ ON A.FK = B.PK [AND 絞り込み条件]

※ デフォルトINNER JOIN (INNERや、OUTERは省略可能)。
※ JOIN後は、USINGで共通キーを紐づける。更に詳細に設定する場合にON句を使用。
※ ON句の[AND 絞り込み条件]は省略可能。
```

#### ■ USINGでの結合
e.g.
```SQL
SELECT
      p.user_id,
      p.product_id,
      p.unit_price,
      sm.category,
      sm.prod_name
FROM
    `prj-test3.bq_trial.pos` AS p
LEFT JOIN
    `prj-test3.bq_trial.shohin_master` AS sm
USING(product_id)
ORDER BY p.product_id;

--                [key]
-- |  |user_id|product_id|unit_price|category|prob_name|
-- | 1|ABC    |         1|       120|くだもの|いちご   |
-- | 2|XYZ    |         2|       200|野菜    |白菜     |
-- | 3|STU    |         2|       200|野菜    |白菜     |
-- | 4|STU    |         3|       160|野菜    |人参     |
-- |15|www    |        11|       210|null    |null     |

-- [shohin_master.csv]には無い値なので、nullが返る
```
#### ■ ON句による条件指定の結合
e.g.
```SQL
SELECT
    p.user_id,
    p.product_id,
    p.unit_price,
    p.quantity,
    sm.category,
    sm.prod_name
FROM
    `prj-test3.bq_trial.pos` AS p
LEFT JOIN
    `prj-test3.bq_trial.shohin_master` AS sm
ON p.product_id = sm.product_id AND p.user_id="ABC" -- ON句で詳細設定しての結合
ORDER BY p.product_id;

--     [条件]    [key]
-- | |user_id|product_id|unit_price|quantity|category|prod_name|
-- |1|ABC    |         1|       120|      10|くだもの |いちご  |
-- |2|XYZ    |         2|       200|       2|null    |null     |
-- |3|STU    |         2|       200|       3|null    |null     |

-- LEFT JOIN での結合のため、左側の全て取得しnullが返ってしまう。
-- この場合は、INNER JOINを使用すると、意図する挙動になる。
INNER JOIN `prj-test3.bq_trial.shohin_master` AS sm
-- | |user_id|product_id|unit_price|quantity|category|prod_name|
-- |1|ABC    |         1|       120|      10|くだもの |いちご  |
-- |2|ABC    |         4|       160|      12|魚       |アジ    |
-- |3|ABC    |         5|       100|       5|肉       |豚肉    |

-- 更にON句では AND で条件を追加もできる。
ON p.product_id = sm.product_id AND p.user_id="ABC" AND sm.category="肉"
     --[条件]-- --[key]--                   --[条件]--
-- | |user_id|product_id|unit_price|quantity|category|prod_name|
-- |1|ABC    |         5|       100|       5|肉      |豚肉     |
-- |2|ABC    |        10|       150|       8|肉      |豚肉     |
```
#### ■結合例外処理（注意）
e.g. 主キー(PK)の重複により要件を満たさないテーブルの結合
```SQL
SELECT
    p.user_id,
    p.product_id,
    p.unit_price,
    p.quantity,
    smb.category,
    smb.prod_name
FROM
    `prj-test3.bq_trial.pos` AS p
LEFT JOIN
    `prj-test3.bq_trial.shohin_master_bad` AS smb
USING(product_id)
WHERE p.product_id = 3
ORDER BY P.product_id;

-- | |user_id|product_id|unit_price|qunantity|category|prod_name |
-- |1|STU    |         3|       160|        8|野菜     |人参     |
-- |2|STU    |         3|       160|        8|野菜     |人参     |
-- |3|WWW    |         3|       160|        5|野菜     |人参     |
-- |4|WWW    |         3|       160|        5|野菜     |人参     |
-- |5|XYZ    |         3|       160|        2|野菜     |人参     |
-- |6|XYZ    |         3|       160|        2|野菜     |人参     |

/****************************************************************
 * (問題点)
 * pos.csvには、product_id＝３番は３レコード存在している。
 * shohin_master_bad.csvには本来PKは一意に存在しなくてはいけないところ、product_id３番が重複していた。
 * その結果、結合後に3レコード取るはずが、倍の6レコード取得されてしまっていた。(重複分が招いた、問題点)
 * 集計時に二重計上による誤差になりかねない💀
 ****************************************************************/
```

ex.【8.5 演習問題1(0:20)】


```SQL
SELECT
    -- sp.purchase_id, sp.date, sp.user_id,
    -- c.gender,
    CASE(c.gender)
        WHEN 1 THEN "mele"
        WHEN 2 THEN "female"
        ELSE "unknow"
    END AS gender,
    -- CONCAT(c.last_name, " ", c.first_name) AS full_name,
    COUNT(*) AS shop_count,
    SUM(sp.quantity) AS shop_quantity,
    SUM(sp.sales_amount) AS total_amount,
    ROUND(SUM(sp.sales_amount)/SUM(sp.quantity)) AS avg_amount
FROM `prj-test3.bq_sample.shop_purchases` AS sp
LEFT JOIN `prj-test3.bq_sample.customers` AS c
USING(user_id)
GROUP BY gender
ORDER BY total_amount DESC;

-- | |gender|shop_count|shop_quantity|total_amount|avg_amount|
-- |1|female|       792|         1819|    11456055|    6298.0|
-- |2|male  |       450|          984|     7969220|    8099.0|
-- |3|unknow|        40|           92|      693117|    7534.0|

-- #Q1. female
-- #Q2. female
-- #Q3. female
-- #Q4. male
```
ex.【8.5 演習問題2(3:50)】


```SQL
#(miss_code)
-- SELECT
--     -- *,
--     -- sp.user_id,
--     -- ("2018-12-31"- c.birthday)/365 AS age,
--     ROUND(DATE_DIFF(date"2018-12-31", c.birthday, DAY)/365) AS age,
--     CASE(c.gender)
--         WHEN 1 THEN "mele"
--         WHEN 2 THEN "female"
--         ELSE "unknow"
--     END AS gender,
--     ROUND(AVG(quantity),1) AS avg_quantity
-- FROM `prj-test3.bq_sample.shop_purchases` AS sp
-- LEFT JOIN `prj-test3.bq_sample.customers` AS c
-- USING(user_id)
-- GROUP BY gender, age
-- HAVING gender != "unknow"
-- ORDER BY 3 DESC
-- LIMIT 3;
-- | |age |gender|avg_quantity|
-- |1|32.0|male  |         3.7|
-- |2|44.0|male  |         3.3|
-- |3|62.0|male  |         3.0|

#(collect_code)
SELECT
    DATE_DIFF("2018-12-31", cu.birthday, YEAR) AS nenrei,
    CASE cu.gender
        WHEN 1 THEN "男性"
        WHEN 2 THEN "女性"
    END AS seibetsu,
    ROUND(AVG(sp.quantity), 1) AS avg_aty
FROM `prj-test3.bq_sample.shop_purchases` AS sp
LEFT JOIN `prj-test3.bq_sample.customers` AS cu
USING(user_id)
WHERE gender <> 3
GROUP BY nenrei, gender
ORDER BY avg_aty DESC
LIMIT 3;

-- | |nenrei |seibetsu|avg_qty|
-- |1|     31|男性    |     3.5|
-- |2|     66|女性    |     3.2|
-- |3|     62|男性    |     3.1|
```
ex.【8.5 演習問題3(7:30)】


```SQL
SELECT
    CONCAT(c.last_name, " ",c.first_name ) AS full_name,
    SUM(sp.sales_amount) AS total_amount
FROM `prj-test3.bq_sample.shop_purchases` AS sp
LEFT JOIN `prj-test3.bq_sample.customers` AS c
USING(user_id)
WHERE
     c.Is_premium = false  --プレミアム会員以外
    AND c.first_name IS NOT NULL
    AND c.last_name IS NOT NULL
GROUP BY full_name
ORDER BY total_amount DESC
LIMIT 3;

-- | |full_name|total_amount|
-- |1|小杉 信貴 |      104500|
-- |2|宗村 良崇 |       60216|
-- |3|和栗 昇悟 |       59400|
```

### | 複数テーブルの結合
e.g. 書式
```SQL
SELECT
FROM [テーブルA] AS A
(INNER|LEFT[OUTER]|RIGHT[OUTER]|FULL[OUTER])JOIN [テーブルB] AS B
→ USING(FK-PKの両テーブルに共通するカラム)
→ ON A.FK = B.PK [AND 絞り込み条件]
(INNER|LEFT[OUTER]|RIGHT[OUTER]|FULL[OUTER])JOIN [テーブルC] AS C
→ USING(FK-PKの両テーブルに共通するカラム)
→ ON A.FK = C.PK [AND 絞り込み条件]
```
ex.【8.6 演習問題1(2:10)】


```SQL
#(miss_code)
-- SELECT
--     s.shop_name,
--     s.chief_name,
--     SUM(sp.sales_amount) AS total_amount,
--     CASE c.gender
--         WHEN 1 THEN "male"
--         WHEN 2 THEN "female"
--         ELSE "unknow"
--     END AS gender
-- FROM `prj-test3.bq_sample.shop_purchases` AS sp
-- LEFT JOIN `prj-test3.bq_sample.customers` AS c USING(user_id)
-- LEFT JOIN `prj-test3.bq_sample.products_master` AS p USING(product_id)
-- LEFT JOIN `prj-test3.bq_sample.shops_master` AS s USING(shop_id)
-- GROUP BY s.shop_name, s.chief_name, gender
-- HAVING gender = female
-- ORDER BY 1, 3 DESC;

#(collect_code)
SELECT
    sm.chief_name AS tencho,
    sm.shop_name AS shop_name,
    CASE cu.gender
        WHEN 1 THEN "male"
        WHEN 2 THEN "female"
    END AS customer_gender,
    COUNT(DISTINCT sp.user_id) AS kyakusuu,
    SUM(sp.sales_amount) AS uriage
FROM
    `prj-test3.bq_sample.shop_purchases` AS sp
LEFT JOIN
    `prj-test3.bq_sample.customers` AS cu ON sp.user_id = cu.user_id
LEFT JOIN
    `prj-test3.bq_sample.products_master` AS pm ON sp.product_id = pm.product_id
LEFT JOIN
    `prj-test3.bq_sample.shops_master` AS sm ON sp.shop_id = sm.shop_id
WHERE cu.gender <> 3
GROUP BY
    tencho,
    sm.shop_name,
    customer_gender
ORDER BY
    customer_gender,
    uriage DESC
;

-- | |tencho     |shop_name  |customer_gnder|kyakusuu|uriage |
-- |1|柳澤 華子  |自由が丘店 |female        |     306|4721503|
-- |2|山下 唐三郎|下北沢店   |female        |     258|4476825|
-- |5|柳澤 華子  |自由が丘店 |male          |     172|4004558|
-- |6|山下 唐三郎|下北沢店   |male          |     145|2713434|
```
ex.【8.6 演習問題2(7:30)】


```SQL
SELECT
    COUNT(*) AS sales_count,
    SUM(sp.sales_amount) AS total_amount,
    s.chief_name AS chief
FROM `prj-test3.bq_sample.shop_purchases` AS sp
LEFT JOIN `prj-test3.bq_sample.customers` AS c USING(user_id)
LEFT JOIN `prj-test3.bq_sample.products_master` AS p USING(product_id)
LEFT JOIN `prj-test3.bq_sample.shops_master` AS s USING(shop_id)
WHERE
    sp.date BETWEEN "2018-03-01" AND "2018-03-31"
    -- DATE_TRUNC(sp.date, month) = "2018-03-01" #別解
    AND c.prefecture != "Tokyo"
    AND c.gender IS NOT NULL
    AND p.prod_gender = "f"
GROUP BY chief
ORDER BY 2 DESC;

-- | |sales_count|total_amount|chief        |
-- |1|          3|      142276|大井谷 みすず|
-- |2|          4|      108905|山下 唐三郎  |
-- |3|          2|       80667|柳澤 華子    |
```
ex.【8.6 演習問題3(10:20)】


```SQL
SELECT
    -- s.shop_name AS shop_name,
    p.prod_name AS product,
    MAX(sp.sales_amount) AS max_amoount,
    MIN(sp.sales_amount) AS min_amount,
    MAX(sp.sales_amount)-MIN(sp.sales_amount) AS diff_amount
FROM `prj-test3.bq_sample.shop_purchases` AS sp
INNER JOIN `prj-test3.bq_sample.customers` AS c USING(user_id)
INNER JOIN `prj-test3.bq_sample.products_master` AS p USING(product_id)
INNER JOIN `prj-test3.bq_sample.shops_master` AS s ON sp.shop_id = s.shop_id AND chief_name = "大井谷　みすず"
WHERE c.Is_premium = TRUE
GROUP BY product
ORDER BY diff_amount DESC
limit 1;

-- | |product      |max_amoount|min_amount|diff_amount|
-- |1|ブラウス 長袖|      73000|     12775|      60225|

#(other code)
FROM `prj-test3.bq_sample.shop_purchases` AS sp
LEFT JOIN `prj-test3.bq_sample.customers` AS cu ON sp.user_id = cu.shop_id
LEFT JOIN `prj-test3.bq_sample.products_master` AS pm ON sp.product_id = pm.product_id
LEFT JOIN `prj-test3.bq_sample.shops_master` AS sm ON sp.shop_id = sm.shop_id
WHERE
    sm.chief_name LIKE "大井谷%"
    AND cu.Is_premium IS TRUE
GROUP BY pm.prod_name
ORDER BY 4 DESC
LIMIT 1;

-- | |product      |max_amoount|min_amount|diff_amount|
-- |1|ブラウス 長袖|      73000|     12775|      60225|
```

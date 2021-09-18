---
date   : 2021-09-17
title  : 【BigQuery】分析入門 - Section7
excerpt: Google BigQuery基本の「き」について。
tags   : ["Google BigQuery", "SQL基本", "分析基本"]
---

## || Section7
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
#    [関数]() OVER(
#       PARTITION BY [分析を行うグループを作る対象のカラム名] (option)
#       ORDER BY [パーテション中での並べ替えを行うカラム名] ASC|DESC (option)
#       WINDOW_FRAME (option)
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


/********************************************************************/
# ■ WINDOW_FRAME
# eg. [ROWS] [BETWEEN] UNBOUNDED PRECEDING [AND] CURRENT ROW
#
# ※ BETWEEN句の始値、終値には様々な指定ができる。
#    CURRENT ROW         : 現在の行
#    UNBOUNDED PRECEDING : パーテションで定義された境界の、最も上の方
#    UNBOUNDED FOLLOWING : パーテションで定義された境界の、最も下の方
#    [int] PRECEDING     : 現在の行から[int]だけ上の行
#    [int] FOLLOWING     : 現在の行から[int]だけ下の行


/**********************
 * ■ [関数] 番号付け
 **********************/
# ※「OVER句」の中には最大３つ記述ができる。
#    ①PARTITIONBY  [使用可]
#    ②ORDERBY      [必須]
#    ③WINDOWFRAME  [使用不可]
#
# ※ 番号付け関数には様々用意されている。
#    RANK()        : パーテションの中の順位を返す
#    ROW_NUMBER()  : パーテションの中の行数を返す
#    CUME_DIST()   :
#    DENSE_RANK()  :
#    NTILE()       :
#    PERCENT_RANK():

/********************************************************************/
# ■ RANK() -
# eg. RANK() OVER(
#           PARTION BY
#           ORDER BY
#     )
#【7.4 演習問題1(2:20)】
SELECT
    order_id,
    user_id,
    quantity,
    RANK() OVER(
        ORDER BY quantity DESC
    ) AS ranking
FROM `prj-test3.bq_trial.pos`
ORDER BY ranking;
-- #| |order_id|user_id|quantity|ranking|
-- #|1|      12|STU    |      12|      1|
-- #|2|       9|ABC    |      12|      1|
-- #|3|       1|ABC    |      10|      3|
-- #|4|       3|ABC    |       8|      4|

#【7.4 演習問題2(4:20)】
SELECT
    order_id,
    user_id,
    quantity,
    RANK() OVER(
        PARTITION BY user_id
        ORDER BY quantity DESC
    ) AS ranking
FROM `prj-test3.bq_trial.pos`
ORDER BY user_id, ranking;
-- --                     [desc]
-- #| |order_id|user_id|quantity|ranking|
-- #|1|       9|ABC    |      12|      1|
-- #|2|       1|ABC    |      10|      2|
-- #|3|       3|ABC    |       8|      3|
-- #|4|      10|ABC    |       6|      4|
-- #|5|       2|ABC    |       5|      5|
-- -- ------------------------------------[partitin]
-- #|6|      12|STU    |      12|      1|

#【7.4 演習問題3(6:20)】
SELECT
    order_id,
    user_id,
    quantity,
    RANK() OVER(
        PARTITION BY user_id
        ORDER BY quantity DESC
    ) AS ranking
FROM `prj-test3.bq_trial.pos`
WHERE ranking <= 3
ORDER BY user_id, ranking;
-- # ∴ WHERE句は使用不可（分析関数では絞り込みできない ∵WHEREが先に実行され、その後にSELECTの為）

/********************************************************************/
# ■ ROW_NUMBER() - パーテションの中の行番号を返す。
# eg. ROW_NUMBER() OVER(
#           PARTITION BY [パーテションを宣言するカラム] (option)
#           ORDER BY [行番号を取得する対象の指標カラム] ASC|DESC(必須)
#     )
#
#【7.4 演習問題4(9:00)】
SELECT
    user_id,
    session_count,
    page,
    ROW_NUMBER() OVER(
        PARTITION BY session_count
        ORDER BY timestamp ASC
    ) AS hit_count
FROM `prj-test3.bq_trial.access_log`;
-- #| |user_id|session_count|page           |hit_count|
-- #|1|ABC    |            2|/products/?id=7|        1|
-- #|2|ABC    |            2|/cart/cart.php |        2|
-- #|3|ABC    |            2|/products/?id=7|        3|
-- -- --------------------------------------------------[partition]
-- #|4|ABC    |            1|/index.php     |        1|


/**************************
 * ■ [関数] ナビゲーション
 **************************/
# ※ 様々なナビゲーション関数
#    FIRST_VALUE()     : ウィンドウフレームの中の一番最初の値を返す
#    LAST_VALUE()      : ウィンドウフレームの中の一番最後の値を返す
#    LEAD()            : 1行、もしくは指定した行数後ろのレコーその値を返す。
#    LAG()             : 1行、もしくは指定した行数前のレコーその値を返す。
#    NTH_VALUE()       :
#    PERCENTILE_COUNT():
#    PERCENTILE_DISC() :

/********************************************************************/
# ■ FIRST_VALUE() -
# eg. FIRST_VALUE(値を取得する対象カラム) OVER(
#           [PARTITION BY     (option)]境界宣言するカラム
#           [ORDER BY(option) ASC|DESC]順序を決める対象カラム
#           [WINDOW FRAME     (option)]
#     )
#【7.5 演習問題1(1:30)】
#(miss_codd)
-- SELECT
--     user_id,
--     product_id,
--     unit_price,
--     quantity,
--     FIRST_VALUE(order_id) OVER(
--         PARTITION BY user_id
--         ORDER BY user_id
--     )
-- FROM `prj-test3.bq_trial.pos`;
#(collect_code)
SELECT
    user_id,
    FIRST_VALUE(product_id) OVER(
        PARTITION BY user_id
        ORDER BY order_id
    ) AS first_purchase_item
FROM `prj-test3.bq_trial.pos`;
-- #| |user_id|first_purchase_item|
-- #|1|ABC    |                  1|
-- #|2|ABC    |                  1|
-- #|6|STU    |                  3|
-- #|9|www    |                  3|

#【7.5 演習問題2(4:10)】
SELECT
    user_id,
    session_count,
    FIRST_VALUE(page) OVER(
        PARTITION BY session_count
        ORDER BY timestamp ASC
    ) AS landing_page,
    LAST_VALUE(page) OVER(
        PARTITION BY session_count
        ORDER BY timestamp ASC
    ) AS exit_page
FROM `prj-test3.bq_trial.access_log`;
-- #| |user_id|session_count|landing_page   |exit_page       |
-- #|1|ABC    |            1|/index.php     |/index.php      |
-- #|2|ABC    |            1|/index.php     |/special/       |
-- #|5|ABC    |            1|/index.php     |/products/?id=10|
-- -- ---------------------------------------------------------[PARTITION]
-- #|6|ABC    |            2|/products/?id=7|/products/?id=7 |
-- #
-- # ※（問題）exit_pageの項目がバラバラ問題
-- #          ∵ [WINDOW FRAME]がデフォルトの値が入ってしまっているから。（設定しないと入ってしまう。）
-- #            [ROWS] [BETWEEN] UNBOUNDED PRECEDING [AND] CURRENT ROW
-- #            その為、パーテションの中での最初と最後の値を取得しているのではなく、
-- #            ウィンドウの中での最初の値、最後の値を取得している。

#【7.5 演習問題3(4:10)】
SELECT
    user_id,
    session_count,
    FIRST_VALUE(page) OVER(
        PARTITION BY session_count
        ORDER BY timestamp ASC
    ) AS landing_page,
    LAST_VALUE(page) OVER(
        PARTITION BY session_count
        ORDER BY timestamp ASC
        ROWS BETWEEN UNBOUNDED PRECEDING  AND UNBOUNDED FOLLOWING
    ) AS exit_page
FROM `prj-test3.bq_trial.access_log`;
-- #| |user_id|session_count|landing_page   |exit_page          |
-- #|1|ABC    |            1|/index.php     |/products/?id=10   |
-- #|2|ABC    |            1|/index.php     |/products/?id=10   |
-- #|5|ABC    |            1|/index.php     |/products/?id=10   |
-- -- ------------------------------------------------------------[PARTITION]
-- #|6|ABC    |            2|/products/?id=7|//thanks/thanks.php|

/********************************************************************/
# ■ LEAD(), LAG() -
# LEAD() - 自分の今いる行の１行下の行の値を取得
# LAG()  - 自分の今いる行の１行下の上の値を取得
# eg. [関数](値を取得する対象カラム, [int]行後, 値がな場合のデフォルト値) OVER(
#        [PARTITION BY]
#        [ORDER BY]
#        [WINDOW FRAME]
#    )
#
#【7.5 演習問題4(13:20)】
#(miss code)
-- SELECT
--     page,
--     DATETIME_DIFF(
--         LEAD() OVER(
--             PARTITION BY
--             ORDER BY
--         ),
--         LEAD() OVER(
--             PARTITION BY
--             ORDER BY
--         ),
--         second) AS time_on_page
-- FROM `prj-test3.bq_trial.access_log`;
#(collect_code)
SELECT
    user_id,
    session_count,
    page,
    timestamp,
    LEAD(page) OVER(                --次の行のpageを取ってきて！
        PARTITION BY session_count
        ORDER BY timestamp
    ) AS next_page,
    LEAD(timestamp) OVER(           --次の行のtimestampを取ってきて！
        PARTITION BY session_count
        ORDER BY timestamp
    ) AS next_hit_timestamp,
    DATETIME_DIFF(
        LEAD(timestamp) OVER(
            PARTITION BY session_count
            ORDER BY timestamp
        ),
        timestamp,
        second
    ) AS time_on_page
FROM `prj-test3.bq_trial.access_log`
ORDER BY timestamp;
-- #| |user_id|session_count|page              |timestamp              |next_page         |next_hit_timestamp     |time_on_page|
-- #|1|ABC    |            1|/index.php        |2018-12-30 14:51:18 UTC|/special/         |2018-12-30 14:53:05 UTC|         107|
-- #|2|ABC    |            1|/special/         |2018-12-30 14:53:05 UTC|/products/?id=1   |2018-12-30 15:00:02 UTC|         417|
-- #|3|ABC    |            1|/products/?id=1   |2018-12-30 15:00:02 UTC|/products/?id=7   |2018-12-30 15:00:30 UTC|          28|
-- #|4|ABC    |            1|/products/?id=7   |2018-12-30 15:00:30 UTC|/products/?id=10  |2018-12-30 15:09:52 UTC|         562|
-- #|5|ABC    |            1|/products/?id=10  |2018-12-30 15:09:52 UTC|null              |null                   |        null| ←離脱
-- #|6|ABC    |            2|/products/?id=7   |2019-01-03 16:04:32 UTC|/cart/cart.php    |2019-01-03 16:05:11 UTC|          39|
-- #|7|ABC    |            2|/cart/cart.php    |2019-01-03 16:05:11 UTC|/thanks/thanks.php|2019-01-03 16:06:09 UTC|          58|
-- #|7|ABC    |            2|/thanks/thanks.php|2019-01-03 16:06:09 UTC|null              |null                   |        null| ←離脱


/**********************
 * ■ [関数] 集計分析
 **********************/
# ※ 様々な集計分析関数
#    SUM()  : パーテションの中の合計値を返す
#    AVG()  : パーテションの中の平均値を返す
#    COUNT(): パーテションの中の値の個数を返す
#    MAX()  : パーテションの中の最大値を返す
#    MIN()  : パーテションの中の最小値を返す
# eg. [関数](値を取得する対象カラム) OVER(
#        PARTITION BY (option)
#        ORDER BY     (option)
#        WINDOW FRAME (option)
#    )
#【7.6 演習問題1(1:20)】
SELECT
    user_id,
    order_id,
    quantity,
    SUM(quantity) OVER(
        PARTITION BY user_id
        ORDER BY order_id
        -- (ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
    ) AS cumulative_ttl
FROM `prj-test3.bq_trial.pos`;
-- #| |user_id|order_id|quantity|cumulative_ttl|
-- #|1|ABC    |       1|      10|            10|
-- #|2|ABC    |       2|       5|            15|
-- #|3|ABC    |       3|       8|            23|
-- #|4|ABC    |       9|      12|            35|

#【7.6 演習問題2(3:10)】
#(miss_code)
-- SELECT
--     user_id,
--     unit_price,
--     AVG(unit_price) OVER(
--         PARTITION BY user_id
--         ORDER BY order_id
--         ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING
--     ) AS avg_amount_3moving
-- FROM `prj-test3.bq_trial.pos`;

-- -- SELECT (120+100+150)/3;
#(collect_code)
SELECT
    user_id,
    order_id,
    quantity,
    unit_price,
    quantity * unit_price AS sales_amount,
    ROUND(AVG(quantity * unit_price) OVER(
        PARTITION BY user_id
        ORDER BY order_id
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW --2行行前から現在の行まで
    ), 2) AS avg_amount_3moving
FROM `prj-test3.bq_trial.pos`;
-- #| |user_id|order_id|quantity|unti_price|sales_amount|avg_amount_3moving|
-- #|1|ABC    |       1|      10|       120|        1200|            1200.0|
-- #|2|ABC    |       2|       5|       100|         500|             850.0| --(1200+500)/2
-- #|3|ABC    |       3|       8|       150|        1200|            966.77| --(1200+500+1200)/3
-- #|4|ABC    |       9|      12|       160|        1920|           1206.67| --(500+1200+1920)/3
-- #|5|ABC    |      10|       6|       200|        1200|            1440.0| --(1200+1920+1200)/3

#【7.6 演習問題3(6:50)】
SELECT
    user_id, order_id, quantity,
    unit_price * quantity AS total_price,
    MAX(unit_price * quantity) OVER(
        PARTITION BY user_id
        ORDER BY order_id
        -- ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS lagest_qty_ever
FROM `prj-test3.bq_trial.pos`;
-- #| |user_id|order_id|quantity|total_sales|lagest_qty_ever|
-- #|1|ABC    |       1|      10|       1200|           1200|
-- #|2|ABC    |       2|       5|        500|           1200|
-- #|3|ABC    |       3|       8|       1200|           1200|
-- #|4|ABC    |       9|      12|       1920|           1920|


```

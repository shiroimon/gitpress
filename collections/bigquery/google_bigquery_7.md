---
date    : 2021-09-07
title   : 🔍 分析入門 
excerpt : - Section7: 分析関数
tags    : ["Google BigQuery", "SQL", "分析", "window関数", "Udemy講座"]
---

## || Section7
### | 分析関数(ウィンドウ関数)
分析関数（ウィンドウ関数）とは、行レベル関数、集計関数よりも、複雑で高度な計算ができる関数の種類。
「番号付け関数」「ナビゲーション関数」「集計関数」の3つに大別することができる。

どんな時に使うの？

    ・店舗ごとに販売されたそれぞれの商品が、売上順で何位にランキングされるか求めたい。
    ・Webアクセスログから、ランディングページ毎のセッション数を求めたり、セッション開始時刻を終了時刻の差分からセッション時間を求め流ような場合。


### | OVER句 - 分析関数の書式
※ SQLでの分析において最も重要な句。内側で3つの` (option)`がある。
  ウィンドウ関数は、数式やWHERE句で用いることができない。
  これは、ウィンドウ関数がWHERE句やGROUPBY句などの処理が終わった結果に対して作用する為。

```SQL
[関数]() OVER(
    (option.1) PARTITION BY {分析を行うグループを作る対象のカラム名}
    (option.2) ORDER BY {パーテション中での並べ替えを行うカラム名} ASC|DESC
    (option.3) {WINDOW_FRAME}
) AS 別名
```


e.g.
```SQL
SELECT
    user_id,
    order_id,
    quantity,
    SUM(quantity) OVER(
        --PARTITION BY句： user_id別にまとめる
        PARTITION BY user_id
        --ORDER BY句：patation内でquantityが多い順
        ORDER BY quantity DESC
        --WINDOW_FRAME句: 境界の最上の値から現在の行まで
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS cumlative_quantity_till_the_row
FROM `prj-test3.bq_trial.pos`
GROUP BY
    user_id,
    order_id,
    quantity;

--                      〈desc〉
-- | |user_id|order_id|quantity|cumlative_quantity_till_the_row| --境界の無い当該行までの累計数量取得
-- |1|ABC    |       9|      12|                             12|
-- |2|ABC    |       1|      10|                             22|←(12+10)
-- |3|ABC    |       3|       8|                             30|←(12+10+8)
-- |4|ABC    |      10|       6|                             36|←(12+10+8+6)
-- |5|ABC    |       2|       5|                             41|←(12+10+8+6+5)
---------------------------------------------------------------------〈patition〉
-- |6|STU    |      12|      12|                             12|
-- |7|STU    |       4|       8|                             20|←(12+8)
-- |8|STU    |      11|       3|                             23|←(12+8+3)
---------------------------------------------------------------------〈patition〉
-- |9|www    |       5|       5|                              5|
```

### | (option.1) PARTITION BY句
SELECT句における、GROUPBYのようにグループ分けを行う。
GRUOPBY句に似ているが、PARTITIONBY句では行の「集約」は行われない。

PARTIONによって分割されたまとまりを其々ウィンドウと呼ぶ。

e.g.
```sql
SELECT
    item_name,
    item_category,
    price,
    SUM(price) OVER(PARTITION BY item_category) AS sum_category
FROM
    item_purchase_log
;
```
|item_name|item_category|price|sum_category|
|-        |-            |-:   |-:          |
|コーヒー |beverrage    |280  |1780        |
|コーヒー |beverrage    |750  |1780        |
|コーヒー |beverrage    |750  |1780        |
|紅茶     |food         |280  |1860        |
|緑茶     |food         |200  |1860        |
|砂糖     |food         |200  |1860        |
|メイプル |food         |450  |1860        |
|メイプル |food         |450  |1860        |
|砂糖     |food         |280  |1860        |


### | (option.2) ORDER BY句
(割愛)

### | (option.3) WINDOW FRAME句

```sql
ROWS BETWEEN {UNBOUNDED PRECEDING} AND {CURRENT ROW}

-- ※ 未指定の場合上記がデフォで入る。
```
```
※ BETWEEN句の始値、終値には様々な指定ができる。
   CURRENT ROW         : 現在の行
   UNBOUNDED PRECEDING : パーテションで定義された境界の、最も上の方
   UNBOUNDED FOLLOWING : パーテションで定義された境界の、最も下の方
   [int] PRECEDING     : 現在の行から[int]だけ上の行
   [int] FOLLOWING     : 現在の行から[int]だけ下の行
```
e.g.
```SQL
-- パーティション全体
BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
-- パーティションの最上部から現在の行まで。（sumと併用すると、累計を取得可）
BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
-- 現在行を含む、直上７レコード。（ORDERBYを日付順にし、avg併用すると直近7日間の移動平均が取得可能）
BETWEEN 6 PRECEDING AND CURRENT ROW
```

※ ROWS以外の指定もできる。(本コースでは扱わないとのこと)
```sql
RANGE BETWEEN {UNBOUNDED PRECEDING} AND {CURRENT ROW}
```

### | [関数]（）
#### ■  番号付け [関数]（）
使える`option`:
```
① PARTITIONBY  [使用可]
② ORDERBY      [必須]
③ WINDOWFRAME  [使用不可]
```
e.g.
```SQL
SELECT
    item_name,
    item_category,
    price,
    RANK() OVER(ORDER BY price) AS ranking,
    DENSE_RANK() OVER(ORDER BY price) AS dense_ranking,
    ROW_NUMBER() OVER(ORDER BY price) AS row_num,
FROM
    item_purchase_log
;
-- | |item_name|item_category|price|ranking|dense_ranking|row_num|
-- |1|緑茶     |food         |  200|      1|            1|      1|
-- |2|砂糖     |food         |  200|      1|            1|      2|
-- |3|コーヒー |beverrage    |  280|      3|            2|      3|
-- |4|緑茶     |food         |  280|      3|            2|      4|
-- |5|砂糖     |food         |  200|      3|            2|      5|
-- |6|メイプル |food         |  450|      6|            3|      6|
-- |7|メイプル |food         |  450|      6|            3|      7|
-- |8|コーヒー |beverrage    |  750|      8|            4|      8
```
※ 番号付け関数には様々用意されている。
```
▷RANK()        : パーテションの中の順位を返す（同率は飛ばす）
  DENSE_RANK()  : パーテションの中の順位を返す（同率は飛ばさない）
  PERCENT_RANK():
▷ROW_NUMBER()  : パーテションの中の行数を返す
  CUME_DIST()   :
  NTILE()       :
```

##### | RANK() - ランキング
e.g.
```SQL
RANK() OVER(
      PARTION BY {hoge}
      ORDER BY {fuga}
) AS {hage}
```

ex.【7.4 演習問題1(2:20)】

trialデータセットのposテーブルに対し、パーティションを宣言しないで、order_id、user_id、qquantity及び、qunantity降順に基づくランクを取得。ランキングは昇順に並べ替えた結果を返す。（ランキングを表示するカラムは別名でrankを付与）
```SQL
SELECT
    order_id,
    user_id,
    quantity,
    RANK() OVER(ORDER BY quantity DESC) AS rank
FROM `prj-test3.bq_trial.pos`
ORDER BY rank;

-- | |order_id|user_id|quantity|rank|
-- |1|      12|STU    |      12|   1|
-- |2|       9|ABC    |      12|   1|
-- |3|       1|ABC    |      10|   3|
-- |4|       4|STU    |       8|   4|
-- |5|       3|ABC    |       8|   4|
```
ex.【7.4 演習問題2(4:20)】

trialデータセットのposテーブルに対し、user_id別にquantityの多い順にランク値を取得。
並べ替えは、user_id、ランク値の順にどちらも昇順で行う。取得カラムはorder_id、user_id、quantityランク値（別名をrankとする）。
```SQL
SELECT
    order_id,
    user_id,
    quantity,
    RANK() OVER(
        PARTITION BY user_id
        ORDER BY quantity DESC
    ) AS rank
FROM `prj-test3.bq_trial.pos`
ORDER BY user_id, rank;

--                       [desc]
-- | |order_id|user_id|quantity|ranking|
-- |1|       9|ABC    |      12|      1|
-- |2|       1|ABC    |      10|      2|
-- |3|       3|ABC    |       8|      3|
-- |4|      10|ABC    |       6|      4|
-- |5|       2|ABC    |       5|      5|
-- -------------------------------------- [partitin]
-- |6|      12|STU    |      12|      1|
-- |7|       4|STU    |       8|      2|
```
ex.【7.4 演習問題3(6:20)】

更に、rankが3位以内という絞り込みを行う。
```SQL
SELECT
    order_id,
    user_id,
    quantity,
    RANK() OVER(
        PARTITION BY user_id
        ORDER BY quantity DESC
    ) AS rank
FROM `prj-test3.bq_trial.pos`
WHERE rank <= 3
ORDER BY user_id, rank;

-- ∴WHERE句は使用不可（分析関数では絞り込みできない）
-- ∵WHEREが先に実行され、その後にSELECTの為
-- (解決)サブクエリを用いる
```

##### | ROW_NUMBER() - パーテションの中の行番号を返す。
e.g.
```SQL
ROW_NUMBER() OVER(
      PARTITION BY {hoge}
      ORDER BY {fuga} ASC|DESC
) AS {hage}
```
ex.【7.4 演習問題4(9:00)】

access_logテーブルから、user_idsession_countpage、セッション中のヒットカウント列を取得。
ヒットカウントは同一セッションにおける、何番目のヒットだったか？を表す整数。
```SQL
SELECT
    user_id,
    session_count,
    page,
    ROW_NUMBER() OVER(
        PARTITION BY session_count
        ORDER BY timestamp ASC
    ) AS hit_count
FROM `prj-test3.bq_trial.access_log`;

-- | |user_id|session_count|page            |hit_count|
-- |1|ABC    |            2|/products/?id=7 |        1|
-- |2|ABC    |            2|/cart/cart.php  |        2|
-- |3|ABC    |            2|/products/?id=7 |        3|
-- -- --------------------------------------------------[partition]
-- |4|ABC    |            1|/index.php      |        1|
-- |5|ABC    |            1|/special/       |        2|
-- |6|ABC    |            1|/products/?id=1 |        3|
-- |7|ABC    |            1|/products/?id=7 |        4|
-- |8|ABC    |            1|/products/?id=10|        5|
```

#### ■ ナビゲーション [関数]（）
e.g.
```SQL
[関数](値を取得する対象カラム) OVER(
      PARTITION BY {hoge}
      ORDER BY {fuga}
      {WINDOW FRAME}
) AS {hage}
```

※ 様々なナビゲーション関数
```
FIRST_VALUE()     : ウィンドウフレームの中の一番最初の値を返す
LAST_VALUE()      : ウィンドウフレームの中の一番最後の値を返す
LEAD()            : 1行、もしくは指定した行数後ろのレコーその値を返す。
LAG()             : 1行、もしくは指定した行数前のレコーその値を返す。
NTH_VALUE()       :
PERCENTILE_COUNT():
PERCENTILE_DISC() :
```

##### | FIRST_VALUE() -
e.g.
```SQL
FIRST_VALUE(値を取得する対象カラム) OVER(
      PARTITION BY {hoge}
      ORDER BY {fuga}
      {WINDOWF RAME}
) AS {hage}
```

ex.【7.5 演習問題1(1:30)】

分析関数first_value()を利用して、trialデータセットのposテーブルから、「ユーザー別に初回購入した商品」を取得。購入の順番は、order_idが小さいほど早い。
```SQL
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

-- | |user_id|first_purchase_item|
-- |1|ABC    |                  1|
-- |2|ABC    |                  1|
-- |6|STU    |                  3|
-- |9|www    |                  3|
```
ex.【7.5 演習問題2(4:10)】

trailデータセットのaccess_logテーブルを利用して、user_id、session_count、landing_page（セッション中のヒットを時系列的に並べた時の最初のページ）、exit_page（セッション中のヒットを時系列に並べた時の最後のページ）を取得。
```SQL
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

-- | |user_id|session_count|landing_page   |exit_page       |
-- |1|ABC    |            1|/index.php     |/index.php      |
-- |2|ABC    |            1|/index.php     |/special/       |
-- |5|ABC    |            1|/index.php     |/products/?id=10|
-- -----------------------------------------------------------[PARTITION]
-- |6|ABC    |            2|/products/?id=7|/products/?id=7 |


/***************************************************************
 * ※（問題点）exit_pageの項目がバラバラ問題
 *    ∵ [WINDOW FRAME]がデフォルトの値が入ってしまっているから。
 *       （設定しないと入ってしまう。）
 *
 *  [ROWS] [BETWEEN] UNBOUNDED PRECEDING [AND] CURRENT ROW
 *
 *  その為、パーテションの中での最初と最後の値を取得しているのではなく、
 *  ウィンドウの中での最初の値、最後の値を取得している。
 ***************************************************************/
```
ex.【7.5 演習問題3(4:10)】

前問では、WINDOWFRAMがデフォルトの状態で取得してしまっている。
正しいexit_pageを取得。
```SQL
SELECT
    user_id,
    session_count,
    FIRST_VALUE(page) OVER(
        PARTITION BY session_count
        ORDER BY timestamp ASC
        -- ROWS BETWEEN UNBOUNDED PRECEDING  AND UNBOUNDED FOLLOWING
        ) AS landing_page,
    LAST_VALUE(page) OVER(
        PARTITION BY session_count
        ORDER BY timestamp ASC
        ROWS BETWEEN UNBOUNDED PRECEDING  AND UNBOUNDED FOLLOWING
        ) AS exit_page
FROM `prj-test3.bq_trial.access_log`;

-- | |user_id|session_count|landing_page   |exit_page          |
-- |1|ABC    |            1|/index.php     |/products/?id=10   |
-- |2|ABC    |            1|/index.php     |/products/?id=10   |
-- |5|ABC    |            1|/index.php     |/products/?id=10   |
-- -- ------------------------------------------------------------[PARTITION]
-- |6|ABC    |            2|/products/?id=7|//thanks/thanks.php|
```

##### | LEAD()、 LAG()
```sql
LEAD() - 自分の今いる行の１行下の行の値を取得
LAG()  - 自分の今いる行の１行下の上の値を取得
```
e.g.
```SQL
[関数](値を取得する対象カラム, [int]行後, 値がな場合のデフォルト値) OVER(
    PARTITION BY {hoge}
    ORDER BY {fuga}
    {WINDOW FRAME}
) AS {hage}
```

ex.【7.5 演習問題4(13:20)】

access_logテーブルから、ページ毎の滞在時間（秒数）を、time_on_pageとして取得。
time_on_pageは、あるページがヒットされたdatetimeから、次のページがヒットされたdatetimeを差し引くことで求まる。
```SQL
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
    --次の行のpageを取得
    LEAD(page) OVER(PARTITION BY session_count ORDER BY timestamp) AS next_page,
    --次の行のtimestampを取得
    LEAD(timestamp) OVER(PARTITION BY session_count ORDER BY timestamp) AS next_hit_timestamp,
    DATETIME_DIFF(
        --次の行のtimestampを取得
        LEAD(timestamp) OVER(PARTITION BY session_count ORDER BY timestamp),
        timestamp,
        second
        ) AS time_on_page
FROM `prj-test3.bq_trial.access_log`
ORDER BY timestamp;

-- | |user_id|session_count|page              |timestamp              |next_page         |next_hit_timestamp     |time_on_page|
-- |1|ABC    |            1|/index.php        |2018-12-30 14:51:18 UTC|/special/         |2018-12-30 14:53:05 UTC|         107|
-- |2|ABC    |            1|/special/         |2018-12-30 14:53:05 UTC|/products/?id=1   |2018-12-30 15:00:02 UTC|         417|
-- |3|ABC    |            1|/products/?id=1   |2018-12-30 15:00:02 UTC|/products/?id=7   |2018-12-30 15:00:30 UTC|          28|
-- |4|ABC    |            1|/products/?id=7   |2018-12-30 15:00:30 UTC|/products/?id=10  |2018-12-30 15:09:52 UTC|         562|
-- |5|ABC    |            1|/products/?id=10  |2018-12-30 15:09:52 UTC|null              |null                   |        null| ←離脱
-- |6|ABC    |            2|/products/?id=7   |2019-01-03 16:04:32 UTC|/cart/cart.php    |2019-01-03 16:05:11 UTC|          39|
-- |7|ABC    |            2|/cart/cart.php    |2019-01-03 16:05:11 UTC|/thanks/thanks.php|2019-01-03 16:06:09 UTC|          58|
-- |7|ABC    |            2|/thanks/thanks.php|2019-01-03 16:06:09 UTC|null              |null                   |        null| ←離脱
```

#### ■ 集計分析 [関数]（）
※ 様々な集計分析関数
```
SUM()  : パーテションの中の合計値を返す
AVG()  : パーテションの中の平均値を返す
COUNT(): パーテションの中の値の個数を返す
MAX()  : パーテションの中の最大値を返す
MIN()  : パーテションの中の最小値を返す
```

e.g.
```SQL
[関数](値を取得する対象カラム) OVER(
    PARTITION BY {hoge}
    ORDER BY {fuga}
    {WINDOW FRAME}
) AS {hage}
```

ex.【7.6 演習問題1(1:20)】

集計分析関数sum()を利用して、productsテーブルから、「ユーザー別の購入順累計quantity」を求めcumlative_ttlという別名を付与。購入の順番は、order_idが小さいほど早い。
（Aさんが、購入順に、２個、５個、６個と購入した場合、２、７、１３という累計値を取得したい。）
```SQL
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

-- | |user_id|order_id|quantity|cumulative_ttl|
-- |1|ABC    |       1|      10|            10|
-- |2|ABC    |       2|       5|            15|
-- |3|ABC    |       3|       8|            23|
-- |4|ABC    |       9|      12|            35|
-- |5|ABC    |      10|       6|            41|
-- |6|STU    |       4|       8|             8|
-- |7|STU    |      11|       3|            11|
```
ex.【7.6 演習問題2(3:10)】

集計分析関数avg()を利用して、productsテーブルから、「ユーザー別の直近3回の購入金額の平均」を求めave_amount_3movingという別名を付与。購入順は、order_idが小さいほど早い。
（Aさんが、購入順に、２個、５個、６個、１個と購入した場合、2.0、3.5、4.3、4.0という移動平均値を取得したい。）
```SQL
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
    ROUND(
        AVG(quantity * unit_price) OVER(
            PARTITION BY user_id
            ORDER BY order_id
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW --2行前から現在の行まで
            ), 2) AS avg_amount_3moving
FROM `prj-test3.bq_trial.pos`;

-- | |user_id|order_id|quantity|unti_price|sales_amount|avg_amount_3moving|
-- |1|ABC    |       1|      10|       120|        1200|            1200.0|
-- |2|ABC    |       2|       5|       100|         500|             850.0| (1200+500)/2
-- |3|ABC    |       3|       8|       150|        1200|            966.77| (1200+500+1200)/3
-- |4|ABC    |       9|      12|       160|        1920|           1206.67| (500+1200+1920)/3
-- |5|ABC    |      10|       6|       200|        1200|            1440.0| (1200+1920+1200)/3
```
ex.【7.6 演習問題3(6:50)】

集計分析関数max()を利用して、posテーブルから、「ユーザー別に1回の買い物での過去最大のquantity」を取得し、lagest_qty_everという別名を付与。購入順番はorder_idが小さいほど早い。
（Aさんが、購入回数順に２個、５個、６個、４個と購入した場合、２、５、６、６という数値を取得したい。）
```SQL
SELECT
    user_id,
    order_id,
    quantity,
    unit_price * quantity AS total_price,
    MAX(unit_price * quantity) OVER(
        PARTITION BY user_id
        ORDER BY order_id
        -- ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
        ) AS lagest_qty_ever
FROM `prj-test3.bq_trial.pos`;

-- | |user_id|order_id|quantity|total_sales|lagest_qty_ever|
-- |1|ABC    |       1|      10|       1200|           1200|
-- |2|ABC    |       2|       5|        500|           1200|
-- |3|ABC    |       3|       8|       1200|           1200|
-- |4|ABC    |       9|      12|       1920|           1920|
```

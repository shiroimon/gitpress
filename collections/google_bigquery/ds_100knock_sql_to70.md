---
date    : 2021-11-15
title   : 6１〜7０本ノック
excerpt : 
tags    : ["DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編
### | S-060 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、売上金額合計を**最小値0、最大値1に正規化**して顧客ID、売上金額合計とともに表 示せよ。ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算する こと。結果は10件表示させれば良い。
```SQL
with
     user_amount as (
          select
               customer_id
               , sum(amount) as amount
          from `prj-test3.100knocks.receipt`
          where customer_id not like 'Z%'
          group by customer_id
     )
     , min_max_scaler as (
          select
               *
               , min(amount) over() as min
               , max(amount) over() as max
          from user_amount
     )
select
     customer_id
     , amount
     , (amount - min)/(max - min) as mm_amount
from min_max_scaler
limit 10
;
```
Cf. [正規化（Normalization）／標準化（Standardization）とは？](https://atmarkit.itmedia.co.jp/ait/articles/2110/07/news027.html) -@IT

```
%%sql

WITH sales_amount AS(
    SELECT
        customer_id,
        SUM(amount) as sum_amount
    FROM
        receipt
    WHERE
        customer_id NOT LIKE 'Z%'
    GROUP BY
        customer_id
),
stats_amount AS (
    SELECT
        max(sum_amount) as max_amount,
        min(sum_amount) as min_amount
    FROM
        sales_amount
)
SELECT
    customer_id,
    sum_amount,
    (sum_amount - min_amount) * 1.0
            / (max_amount -  min_amount) * 1.0 AS scale_amount
FROM sales_amount
CROSS JOIN stats_amount
LIMIT 10
```

### | S-061 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、売上金額合計を**常用対数化(底=10)**して顧客ID、売上金額合計とともに表示せ よ。ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算するこ と。結果は10件表示させれば良い。
```SQL
with
     user_amount as (
          select
               customer_id
               , sum(amount) as amount
          from `prj-test3.100knocks.receipt`
          where customer_id not like 'Z%'
          group by customer_id
     )
select
     customer_id
     , amount
     , log10(amount) as log_amount
from user_amount
limit 10
;
```
Cf. [対数(自然対数,常用対数)を求める](https://www.sql-reference.com/math/log.html) - 逆引きSQL構文集

### | S-062 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、売上金額合計を**自然対数化(底=e)**して顧客ID、売上金額合計とともに表示せよ (ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算するこ と)。結果は10件表示させれば良い。
```SQL
with
     user_amount as (
          select
               customer_id
               , sum(amount) as amount
          from `prj-test3.100knocks.receipt`
          where customer_id not like 'Z%'
          group by customer_id
     )
select
     customer_id
     , amount
     , log(amount) as log_amount
from user_amount
limit 10
;
```
```
%%sql

SELECT
    customer_id,
    SUM(amount),
    LN(SUM(amount) + 0.5) as log_amount
FROM
    receipt
WHERE
    customer_id NOT LIKE 'Z%'
GROUP BY
    customer_id
LIMIT 10
```

### | S-063 ★
商品テーブル(product)の単価(unit_price)と原価(unit_cost)から、各商品の利益額 を算出せよ。結果は10件表示させれば良い。
```SQL
select
    *
    , (unit_price - unit_cost) as profit
from `prj-test3.100knocks.product`
limit 10
;
```

### | S-064 ★
商品テーブル(product)の単価(unit_price)と原価(unit_cost)から、各商品の利益率 の全体平均を算出せよ。 ただし、単価と原価にはNULLが存在することに注意せよ。
```SQL
select
    *
    , (unit_price - unit_cost) as profit
    , (unit_price - unit_cost) / unit_price * 100 as profit_rate -- 売上総利益（粗利）
    , avg((unit_price - unit_cost) / unit_price * 100) over () as mean_profit_rate -- 売上総利益（粗利）平均
from `prj-test3.100knocks.product`
limit 10
;
```
Cf.[【超カンタン！】利益率の計算方法・出し方を図解でわかりやすく説明します](https://www.unchi-co.com/kaigyoblog/kigyo_kaigyo/riekiritsu.html) - The企業＆飲食経営
```
%%sql

SELECT
    AVG((unit_price * 1.0 - unit_cost) / unit_price) as unit_profit_rate
FROM
    product
LIMIT 10
```

### | S-065 ★
商品テーブル(product)の各商品について、利益率が30%となる新たな単価を求めよ。 ただし、1円未満は切り捨てること。そして結果を10件表示させ、利益率がおよそ30%付近であることを確認せよ。ただし、単価(unit_price)と原価(unit_cost)にはNULLが 存在することに注意せよ。
```SQL
select
    product_cd
    , unit_cost -- 原価
    , unit_price -- 単価
    , (unit_price - unit_cost) as profit -- 売上総利益（粗利）
    , trunc((unit_price - unit_cost) / unit_price * 100) as profit_rate -- 売上総利益（粗利）率
        /* 新単価(式変換)
         * (p-c) / p  = 0.3
         *      p - c = 0.3p
         *          p = c / 0.7 ∴
         */
    , trunc(unit_cost / 0.7) as new_unit_price -- 新単価
    , trunc((unit_cost / 0.7) - unit_cost) as new_profit -- 新売上総利益（粗利）
    , trunc(((unit_cost / 0.7) - unit_cost) / (unit_cost / 0.7) * 100) as new_profit_rate -- 新売上総利益（粗利）率
from `prj-test3.100knocks.product`
order by product_cd
limit 10
;
```

### | S-066 ★
商品テーブル(product)の各商品について、利益率が30%となる新たな単価を求めよ。 今回は、1円未満を丸めること(四捨五入または偶数への丸めで良い)。そして結果を10 件表示させ、利益率がおよそ30%付近であることを確認せよ。ただし、単価 (unit_price)と原価(unit_cost)にはNULLが存在することに注意せよ。
```SQL
select
    product_cd
    , unit_cost -- 原価
    , unit_price -- 単価
    , (unit_price - unit_cost) as profit -- 売上総利益（粗利）
    , trunc((unit_price - unit_cost) / unit_price * 100) as profit_rate -- 売上総利益（粗利）率
    , round(unit_cost / 0.7) as new_unit_price -- 新単価
    , round((unit_cost / 0.7) - unit_cost) as new_profit -- 新売上総利益（粗利）
    , round(((unit_cost / 0.7) - unit_cost) / (unit_cost / 0.7) * 100) as new_profit_rate -- 新売上総利益（粗利）率
from `prj-test3.100knocks.product`
order by product_cd
limit 10
;
```

### | S-067 ★
商品テーブル(product)の各商品について、利益率が30%となる新たな単価を求めよ。 今回は、1円未満を切り上げること。そして結果を10件表示させ、利益率がおよそ30%付近であることを確認せよ。ただし、単価(unit_price)と原価(unit_cost)にはNULLが 存在することに注意せよ。
```SQL
select
    product_cd
    , unit_cost
    , unit_price
    , ceil(unit_cost / 0.7) as new_unit_price
    , ceil((unit_cost / 0.7) - unit_cost) / ceil(unit_cost / 0.7) as new_unit_price_rate
from `prj-test3.100knocks.product`
limit 10
;
```

### | S-068 ★
商品テーブル(product)の各商品について、消費税率10%の税込み金額を求めよ。1円未 満の端数は切り捨てとし、結果は10件表示すれば良い。ただし、単価(unit_price)にはNULLが存在することに注意せよ。
```SQL
select
    product_cd
    -- , unit_cost
    , unit_price
    , floor(unit_price * 1.1) as tax
from `prj-test3.100knocks.product`
limit 10
;
```

### | S-069 ★★
レシート明細テーブル(receipt)と商品テーブル(product)を結合し、顧客毎に全商品の売上金額合計と、カテゴリ大区分(category_major_cd)が"07"(瓶詰缶詰)の売上金額合計を計算の上、両者の比率を求めよ。抽出対象はカテゴリ大区分"07"(瓶詰缶詰)の 売上実績がある顧客のみとし、結果は10件表示させればよい。
```SQL
with tb as (
    select
        *
    from `prj-test3.100knocks.receipt`
        join `prj-test3.100knocks.product`
            using(product_cd)
)
, user_all_amount as (
    select
        customer_id
        , sum(amount) as amount_all
    from tb
    group by customer_id
)
, user_07_amount as (
    select
        customer_id
        , sum(amount) as amount_07
    from tb
    where category_major_cd = 07
    group by customer_id
)
select
    *
    , amount_07 / amount_all  as ration
from user_all_amount
    join user_07_amount
        using(customer_id)
limit 10
;
```

### | S-070 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、顧客テーブル (customer)の会員申込日(application_date)からの経過日数を計算し、顧客ID (customer_id)、売上日、会員申込日とともに表示せよ。結果は10件表示させれば良い (なお、sales_ymdは数値、application_dateは文字列でデータを保持している点に注意)。
```SQL
select
    customer_id
    , parse_date('%Y%m%d', cast(sales_ymd as string)) as sales_ymd
    , parse_date('%Y%m%d', cast(application_date as string)) as application_date
    , date_diff(
        parse_date('%Y%m%d', cast(sales_ymd as string))
        , parse_date('%Y%m%d', cast(application_date as string))
        , day) as date_diff
from `prj-test3.100knocks.receipt`
    join `prj-test3.100knocks.customer`
        using(customer_id)
limit 10
;
```
Cf. [Bigqueryの日時に関係する関数全部試してみた ①Date編](https://qiita.com/hogeta_/items/1416135863a023a09127)  - Qiita

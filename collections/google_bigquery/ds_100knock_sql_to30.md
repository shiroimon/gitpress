---
date    : 2021-11-15
title   : 🔍 100本ノック
excerpt : 2１〜3０本ノック
tags    : ["🔍", "DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編
### | S-021 ★
レシート明細テーブル(receipt)に対し、件数をカウントせよ。
```SQL
select　count(*)　from　`100knocks.receipt`;
```

### | S-022 ★
レシート明細テーブル(receipt)の顧客ID(customer_id)に対し、ユニーク件数をカウントせよ。
```SQL
select
    count(distinct customer_id)　as unique_cnt
from 
    `100knocks.receipt`
;
```

### | S-023 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)と売上数量(quantity)を合計せよ。
```SQL
select
    store_cd
    , sum(amount) as ttl_amount
    , sum(quantity) as ttl_quantity
from 
    `100knocks.receipt`
group by 
    store_cd
;
```

### | S-024 ★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに最も新しい売上日(sales_ymd)を求め、10件表示せよ。
```sql
select
    customer_id
    , max(recently_sales_ymd) as recently_sales_ymd
from(
    select
        customer_id
        , last_value(sales_ymd) over(
                partition by 
                    customer_id 
                order by 
                    sales_ymd
                rows 
                    between UNBOUNDED PRECEDING and UNBOUNDED FOLLOWING
          ) as recently_sales_ymd
    from 
        `100knocks.receipt`
)
group by 
    customer_id
limit 10
;
```

```
select
    customer_id
    , max(sales_ymd) as recently_sales_ymd
from 
    `100knocks.receipt`
group by 
    customer_id
limit 10
;
```

### | S-025 ★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに最も古い売上日 (sales_ymd)を求め、10件表示せよ。
```SQL
select
    customer_id
    , min(sales_ymd)
from 
    `100knocks.receipt`
group by 
    customer_id
;
```

### | S-026 ★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに最も新しい売上 日(sales_ymd)と古い売上日を求め、両者が異なるデータを10件表示せよ。
```SQL
select
    customer_id
    , max(sales_ymd) as recently
    , min(sales_ymd) as formely
from 
    `100knocks.receipt`
group by 
    customer_id
having 
    recently <> formely
limit 10
;
```

### | S-027 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の平均を計算し、降順でTOP5を表示せよ。
```SQL
select
    store_cd
    , avg(amount)as mean_amount
from
    `100knocks.receipt`
group by 
    store_cd
order by 
    mean_amount desc
limit 5
;
```

### | S-028 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の中央値を計算し、降順でTOP5を表示せよ。
```SQL
# BigQuery
select
    store_cd
    , max(median_amount) as median_amount
from
    (select
         store_cd
         , percentile_cont(amount, 0.5) over(partition by store_cd) as median_amount
     from `100knocks.receipt`) as stcd
group by 
    store_cd
order by 
    median_amount desc
limit 5
;
```
```
# PostgreSQL
select
    store_cd
    , percentile_cont(0.5)within group(order by amount) as median_amount
from `100knocks.receipt`
group by store_cd
order by median_amount desc
limit 5
;
```

### | S-029 ★★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに商品コード (product_cd)の最頻値を求めよ。
```SQL
# BigQuery
select
    store_cd
    , approx_top_count(product_cd, 1) as product_cd_mod
from 
    `100knocks.receipt`
group by 
    store_cd
;
```
Cf.
* [【BigQuery】StandardSQLで最頻値（modeやtop）](https://qiita.com/chatrate/items/e8d3a6cec35dfef4524b) - Qiita
* [BigQueryで平均値、中央値、最頻値をSQLで取得する方法](https://itips.krsw.biz/bigquery-how-to-get-average-median-mode/) - ITips


```
# PostgreSQL（解法１）
WITH product_mode AS (
    SELECT store_cd,product_cd, COUNT(1) as mode_cnt,
        RANK() OVER(PARTITION BY store_cd ORDER BY COUNT(1) DESC) AS rnk
    FROM receipt
    GROUP BY store_cd,product_cd
)
SELECT store_cd,product_cd, mode_cnt
FROM product_mode
WHERE rnk = 1
ORDER BY store_cd,product_cd;

# PostgreSQL（解法２）
SELECT store_cd, mode() WITHIN GROUP(ORDER BY product_cd)
FROM receipt
GROUP BY store_cd
ORDER BY store_cd
```

### | S-030 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の標本分散を計算し、降順でTOP5を表示せよ。
```SQL
# BigQuery 解法1(スクラッチ)
with 
    mean_tb as (
        select
            store_cd
            , avg(amount) as mean
        from 
            `100knocks.receipt`
        group by 
            store_cd
    )
    , diff_tb as (
        select
            store_cd
            , pow(cast(amount as float64) - m.mean, 2) as deviation_square
        from 
            `100knocks.receipt`
            left join mean_tb m using(store_cd)
    )
select
    store_cd
    , avg(deviation_square) as variance
from  diff_tb
group by store_cd
order by variance desc
limit 5
;
```
```sql
# BigQuery 解法2
select
    store_cd
    , variance(amount) as variance
from 
    `100knocks.receipt`
group by 
    store_cd
order by 
    variance desc
limit 5
;
```
Cf. [BigQueryで統計量を出す時に使うクエリメモ](https://qiita.com/hagino3000/items/e9ed62638ebe54391188) - Qiita

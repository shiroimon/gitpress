---
date    : 2021-11-15
title   : 🔍 100本ノック
excerpt : 3１〜4０本ノック
tags    : ["🔍 ", "DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編
### | S-031 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の標本標準偏差を計算し、降順でTOP5を表示せよ。
```SQL
select
    store_cd
    , stddev(amount) as std
from 
    `100knocks.receipt`
group by 
    store_cd
order by 
    std desc
limit 5
;
```

### | S-032 ★
レシート明細テーブル(receipt)の売上金額(amount)について、25%刻みでパーセンタイル値を求めよ。
```SQL
# BigQuery
select
    store_cd
    , max(min) as min
    , max(q1) as q1
    , max(q2) as q2
    , max(q3) as q3
    , max(max) as max
from (
    select
        store_cd
        , percentile_cont(amount, 0.0)  over w as min
        , percentile_cont(amount, 0.25) over w as q1
        , percentile_cont(amount, 0.5)  over w as q2
        , percentile_cont(amount, 0.75) over w as q3
        , percentile_cont(amount, 1.0)  over w as max
    from 
        `100knocks.receipt`
    window
        w as (partition by store_cd)
) as statistic
group by store_cd
;
```
```
# PostgreSQL
SELECT
    PERCENTILE_CONT(0.25) WITHIN GROUP(ORDER BY amount) as amount_25per,
    PERCENTILE_CONT(0.50) WITHIN GROUP(ORDER BY amount) as amount_50per,
    PERCENTILE_CONT(0.75) WITHIN GROUP(ORDER BY amount) as amount_75per,
    PERCENTILE_CONT(1.0) WITHIN GROUP(ORDER BY amount) as amount_100per
FROM receipt
```

### | S-033 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の平均を計算し、330以上のものを抽出せよ。
```SQL
select
    store_cd
    , avg(amount) as mean
from 
    `100knocks.receipt`
group by 
    store_cd
having 
    mean > 330
;
```

### | S-034 ★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに売上金額 (amount)を合計して全顧客の平均を求めよ。ただし、顧客IDが"Z"から始まるのものは 非会員を表すため、除外して計算すること。
```SQL
-- select
--     customer_id
--     , sum(amount) as ttl_amount
-- from `100knocks.receipt`
-- where not regexp_contains(customer_id, r'^Z')
-- group by customer_id
-- ;

with
    customer_tb as(
        select
            customer_id
            , sum(amount) as ttl_amount
        from 
            `100knocks.receipt`
        where 
            not regexp_contains(customer_id, r'^Z')
        group by 
            customer_id
    )
select 
    avg(ttl_amount) 
from 
    customer_tb
;
```

```
# PostgreSQL
WITH customer_amount AS (
    SELECT customer_id, SUM(amount) AS sum_amount
    FROM receipt
    WHERE customer_id not like 'Z%'
    GROUP BY customer_id
)
SELECT AVG(sum_amount) from customer_amount
```

### | S-035 ★★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに売上金額 (amount)を合計して全顧客の平均を求め、平均以上に買い物をしている顧客を抽出せよ。ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算すること。なお、データは10件だけ表示させれば良い。
```SQL
with
    customer_tb as(
        select
            customer_id
            , sum(amount) as ttl_amount
        from 
            `100knocks.receipt`
        where 
            not regexp_contains(customer_id, r'^Z')
        group by 
            customer_id
    )
select
    customer_id
    , ttl_amount
from 
    customer_tb
where 
    ttl_amount > (select avg(ttl_amount) from customer_tb)
limit 10
;
```

### | S-036 ★
レシート明細テーブル(receipt)と店舗テーブル(store)を内部結合し、レシート明細 テーブルの全項目と店舗テーブルの店舗名(store_name)を10件表示させよ。
```SQL
select
    receipt.*
    , store.store_name
from
    `100knocks.receipt` receipt
    left join (select distinct store_cd, store_name from `100knocks.store`) as store using(store_cd)
limit 10
;
```

### | S-037 ★
商品テーブル(product)とカテゴリテーブル(category)を内部結合し、商品テーブル の全項目とカテゴリテーブルの小区分名(category_small_name)を10件表示させよ。
```SQL
select
    *
from
    `100knocks.product` p
left join
    (select category_major_cd, category_medium_cd, category_small_cd, category_small_name from `100knocks.category`) as c
using(category_major_cd, category_medium_cd, category_small_cd)

-- on 
--         p.category_major_cd =c.category_major_cd
--     and p.category_medium_cd=c.category_medium_cd
--     and p.category_small_cd =c.category_small_cd

limit 10
;
```

```
select
    p.*
    , c.category_small_name
from 
    `100knoks.product` p
join 
    `100knoks.category` c using(category_small_cd)
limit 10
;
```

### | S-038 ★
顧客テーブル(customer)とレシート明細テーブル(receipt)から、各顧客ごとの売上 金額合計を求めよ。ただし、売上実績がない顧客については売上金額を0として表示させ ること。また、顧客は性別コード(gender_cd)が女性(1)であるものを対象とし、非会員(顧客IDが"Z"から始まるもの)は除外すること。なお、結果は10件だけ表示させれば良い。
```SQL
-- with customer_amount as(
--     select
--         customer_id
--         , sum(amount) as ttl_amount
--     from `prj-test3.100knocks.receipt`
--     group by customer_id
-- )
-- select
--     customer_id
--     , coalesce(ttl_amount, 0) as ttl_amount
-- from `prj-test3.100knocks.customer`
-- join customer_amount using(customer_id)
-- -- where
-- order by ttl_amount
-- ;

select
    customer_id
    , customer_name
    , sum(amount) as ttl_amount
from 
    `100knocks.customer` c
join 
    `100knocks.receipt` r using(customer_id)
where
        customer_id not like'Z%'
    and gender_cd = 1
group by
    customer_id
    , customer_name
limit 10
;
```
Cf. [SQL関数coalesceの使い方と読み方](https://spirits.appirits.com/doruby/8666/) - Sppirits

### | S-039 ★
レシート明細テーブル(receipt)から売上日数の多い顧客の上位20件と、売上金額合計の多い顧客の上位20件を抽出し、完全外部結合せよ。ただし、非会員(顧客IDが"Z"から始まるもの)は除外すること。
```SQL
select
    customer_id
    , count(distinct sales_ymd) as days
    , sum(amount) as amount
from 
    `100knocks.receipt`
where 
    customer_id not like 'Z%'
group by 
    customer_id
order by  
    days desc  
    , amount desc
limit 20
;
```

### | S-040 ★
全ての店舗と全ての商品を組み合わせると何件のデータとなるか調査したい。店舗 (store)と商品(product)を**直積**した件数を計算せよ。
```SQL
-- select
--     (select
--         count(distinct store_cd)
--     from `100knocks.store`)
--     union all
--     (select
--         count(distinct product_cd)
--     from `100knocks.product`) as ttl_record

select
    count(*)
from 
    `100knocks.store`
cross join 
    `100knocks.product`
;
```
[cross join を知ると join が書きやすくなるよ、という話](https://developer.feedforce.jp/entry/2019/03/19/170000) -  Feedforce Developer Blog

---
date    : 2021-11-15
title   : 挑戦
excerpt : 
tags    : ["DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編

### | 各テーブル確認
```SQL
-- 各テーブルのデータ数確認
select
    *
from(
    select
        'category' as tb_name
        , (select count(*) from `prj-test3.100knocks.category`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'category'
    union all
    select
        'customer' as tb_name
        , (select count(*) from `prj-test3.100knocks.customer`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'customer'
    union all
    select
        'geocode' as tb_name
        , (select count(*) from `prj-test3.100knocks.geocode`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'geocode'
    union all
    select
        'product' as tb_name
        , (select count(*) from `prj-test3.100knocks.product`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'product'
    union all
    select
        'receipt' as tb_name
        , (select count(*) from `prj-test3.100knocks.receipt`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'receipt'
    union all
    select
        'store' as tb_name
        , (select count(*) from `prj-test3.100knocks.store`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'store'
)
order by reco desc
;
```
![img](https://i.gyazo.com/efee5ae5565e1299e43fe26507b6d738.png)

### | S-001 ★
レシート明細テーブル(receipt)から全項目を10件抽出し、どのようなデータを保有して いるか目視で確認せよ。
```SQL
select * from `100knocks.receipt` limit 10;
```

### | S-002 ★
レシート明細のテーブル(receipt)から売上日(sales_ymd)、顧客ID (customer_id)、商品コード(product_cd)、売上金額(amount)の順に列を指定し、 10件表示させよ。
```SQL
select
    sales_ymd
    , customer_id
    , product_cd
    , amount
from `100knocks.receipt`
limit 10
;
```

### |S-003 ★
レシート明細のテーブル(receipt)から売上日(sales_ymd)、顧客ID (customer_id)、商品コード(product_cd)、売上金額(amount)の順に列を指定し、10件表示させよ。ただし、sales_ymdはsales_dateに項目名を変更しながら抽出するこ と。
```SQL
select
    sales_ymd as sales_date
    , customer_id
    , product_cd
    , amount
from `100knocks.receipt`
limit 10
;
```

### | S-004 ★
レシート明細のテーブル(receipt)から売上日(sales_ymd)、顧客ID (customer_id)、商品コード(product_cd)、売上金額(amount)の順に列を指定し、 以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"

```SQL
select
    sales_ymd
    , customer_id
    , product_cd
    , amount
from `100knocks.receipt`
where customer_id = 'CS018205000001'
;
```

### | S-005 ★
レシート明細のテーブル(receipt)から売上日(sales_ymd)、顧客ID (customer_id)、商品コード(product_cd)、売上金額(amount)の順に列を指定し、 以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上

```sql
select
    sales_ymd
    , customer_id
    , product_cd
    , amount
from `100knocks.receipt`
where
    customer_id = 'CS018205000001'
    and
    amount >= 1000
;
```

### | S-006 ★
レシート明細テーブル(receipt)から売上日(sales_ymd)、顧客ID(customer_id)、 商品コード(product_cd)、売上数量(quantity)、売上金額(amount)の順に列を指定 し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上または売上数量（quantity）が5以上

```sql
select
    sales_ymd
    , customer_id
    , product_cd
    , quantity
    , amount
from `100knocks.receipt`
where
    customer_id = 'CS018205000001'
    and
    (amount >= 1000 or quantity >= 5)
;
```

### | S-007 ★
レシート明細のテーブル(receipt)から売上日(sales_ymd)、顧客ID (customer_id)、商品コード(product_cd)、売上金額(amount)の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上2,000以下

```sql
select
    sales_ymd
    , customer_id
    , product_cd
    , amount
from `100knocks.receipt`
where
    customer_id = 'CS018205000001'
    and
    amount between 1000 and 2000
;
```

### | S-008 ★
レシート明細テーブル(receipt)から売上日(sales_ymd)、顧客ID(customer_id)、 商品コード(product_cd)、売上金額(amount)の順に列を指定し、以下の条件を満た すデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 商品コード（product_cd）が"P071401019"以外

```SQL
select
    sales_ymd
    , customer_id
    , product_cd
    , amount
from `100knocks.receipt`
where
    customer_id = 'CS018205000001'
    and
    product_cd <> 'P071401019'
;
```

### | S-009 ★
以下の処理において、出力結果を変えずにORをANDに書き換えよ。

`select * from store where not (prefecture_cd = '13' or floor_area > 900)`

```sql
select
    *
from `100knocks.store`
where
    prefecture_cd <> 13
    and
    floor_area < 900
;
```

### | S-010 ★
店舗テーブル(store)から、店舗コード(store_cd)が"S14"で始まるものだけ全項目抽 出し、10件だけ表示せよ。

```SQL
select
    *
from `100knocks.store`
where store_cd like 'S14%'
-- where regexp_contains(store_cd, r'S14') #別解
limit 10
;
```

### | S-011 ★
顧客テーブル(customer)から顧客ID(customer_id)の末尾が1のものだけ全項目抽出 し、10件だけ表示せよ。

```SQL
select
    *
from `100knocks.customer`
where regexp_contains(customer_id, r'1$')
limit 10
;
```

### | S-012 ★
店舗テーブル(store)から横浜市の店舗だけ全項目表示せよ。
```SQL
select
    *
from `100knocks.store`
where address like '%横浜市%'
;
```

### | S-013 ★★
顧客テーブル(customer)から、ステータスコード(status_cd)の先頭がアルファベッ トのA〜Fで始まるデータを全項目抽出し、10件だけ表示せよ。
```SQL
select
    *
from `100knocks.customer`
where
    regexp_contains(customer_id, r'^(A|B|C|D|E|F)') -- r'^(A-F)'
    -- customer_id like 'A%'
    -- or
    -- customer_id like 'B%'
    -- or
    -- customer_id like 'C%'
    -- or
    -- customer_id like 'D%'
    -- or
    -- customer_id like 'E%'
    -- or
    -- customer_id like 'F%'
limit 10
;
```
Cf. [BigQueryでLIKE文の複数条件指定をORから正規表現に直す](https://qiita.com/mida12251141/items/27fe42ef01e83d5b063f) - Qiita

### | S-014 ★★
顧客テーブル(customer)から、ステータスコード(status_cd)の末尾が数字の1〜9で 終わるデータを全項目抽出し、10件だけ表示せよ。
```SQL
select
    *
from `prj-test3.100knocks.customer`
where
    regexp_contains(status_cd, r'[1-9]$')
limit 10
;
```

### | S-015 ★★
顧客テーブル(customer)から、ステータスコード(status_cd)の先頭がアルファベットのA〜Fで始まり、末尾が数字の1〜9で終わるデータを全項目抽出し、10件だけ表示せよ。
```SQL
select
    *
from `prj-test3.100knocks.customer`
where
    regexp_contains(status_cd, r'^(A|B|C|D|E|F)-\d+-[1-9]$')
limit 10
;
```
Cf. [基本的な正規表現一覧](https://murashun.jp/article/programming/regular-expression.html) - murashun.jp

### | S-016 ★★
店舗テーブル(store)から、電話番号(tel_no)が3桁-3桁-4桁のデータを全項目表示せよ。
```SQL
select
    *
from `prj-test3.100knocks.store`
where regexp_contains(tel_no, r'\d{3}-\d{3}-\d{4}')
;
```

### | S-017 ★
顧客テーブル(customer)を生年月日(birth_day)で高齢順にソートし、先頭10件を全項目表示せよ。
```SQL
select
    *
from `prj-test3.100knocks.customer`
order by birth_day
limit 10
;
```

### | S-018 ★
顧客テーブル(customer)を生年月日(birth_day)で若い順にソートし、先頭10件を全 項目表示せよ。
```SQL
select
    *
from `prj-test3.100knocks.customer`
order by birth_day desc
limit 10
;
```

### | S-019 ★★
レシート明細テーブル(receipt)に対し、1件あたりの売上金額(amount)が高い順にラ ンクを付与し、先頭10件を抽出せよ。項目は顧客ID(customer_id)、売上金額 (amount)、付与したランクを表示させること。なお、売上金額(amount)が等しい場合は同一順位を付与するものとする。
```SQL
select
    customer_id
    , amount
    , rank()over(
            order by amount desc
      ) as ranking
from `prj-test3.100knocks.receipt`
order by ranking
limit 10
;
```

### | S-020 ★★
レシート明細テーブル(receipt)に対し、1件あたりの売上金額(amount)が高い順にラ ンクを付与し、先頭10件を抽出せよ。項目は顧客ID(customer_id)、売上金額 (amount)、付与したランクを表示させること。なお、売上金額(amount)が等しい場 合でも別順位を付与すること。
```SQL
select
    customer_id
    , amount
    , dense_rank()over( -- row_number()
            order by amount desc
      ) as ranking
from `prj-test3.100knocks.receipt`
order by ranking
limit 10
;
```

Cf. [【BIGQUERY】分析入門 - SECTION7](https://gitpress.io/c/google_bigquery/google_bigquery_7)- .tk

### | S-021 ★
レシート明細テーブル(receipt)に対し、件数をカウントせよ。
```SQL
select
    count(*)
from `prj-test3.100knocks.receipt`
;
```

### | S-022 ★
レシート明細テーブル(receipt)の顧客ID(customer_id)に対し、ユニーク件数をカウ ントせよ。
```SQL
select
    count(distinct customer_id)
from `prj-test3.100knocks.receipt`
;
```

### | S-023 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)と売上数量(quantity)を合計せよ。
```SQL
select
    store_cd
    , sum(amount) as ttl_amount
    , sum(quantity) as ttl_quantity
from `prj-test3.100knocks.receipt`
group by store_cd
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
            partition by customer_id
            order by sales_ymd
            rows between unbounded preceding  and unbounded following
        ) as recently_sales_ymd
    from `prj-test3.100knocks.receipt`
)
group by customer_id
limit 10
;
```

```
select
    customer_id
    , max(sales_ymd) as recently_sales_ymd
from `prj-test3.100knocks.receipt`
group by customer_id
limit 10
;
```

### | S-025 ★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに最も古い売上日 (sales_ymd)を求め、10件表示せよ。
```SQL
select
    customer_id
    , min(sales_ymd)
from `prj-test3.100knocks.receipt`
group by customer_id
;
```

### | S-026 ★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに最も新しい売上 日(sales_ymd)と古い売上日を求め、両者が異なるデータを10件表示せよ。
```SQL
select
    customer_id
    , max(sales_ymd) as recently
    , min(sales_ymd) as formely
from `prj-test3.100knocks.receipt`
group by customer_id
having recently <> formely
limit 10
;
```

### | S-027 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の平均を計算し、降順でTOP5を表示せよ。
```SQL
select
    store_cd
    , avg(amount)as mean_amount
from `prj-test3.100knocks.receipt`
group by store_cd
order by mean_amount desc
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
    from `prj-test3.100knocks.receipt`)
group by store_cd
order by median_amount desc
limit 5
;
```
```
# PostgreSQL
select
    store_cd
    , percentile_cont(0.5)within group(order by amount) as median_amount
from `prj-test3.100knocks.receipt`
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
from `prj-test3.100knocks.receipt`
group by store_cd
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
with mean_tb as (
        select
            store_cd
            , avg(amount) as mean
        from `prj-test3.100knocks.receipt`
        group by store_cd
    )
    , diff_tb as (
        select
            store_cd
            , pow(cast(amount as float64) - m.mean, 2) as deviation_square
        from `prj-test3.100knocks.receipt`
        left join mean_tb as m
        using(store_cd)
    )
select
    store_cd
    , avg(deviation_square) as variance
from diff_tb
group by store_cd
order by variance desc
limit 5
;
```
```
# BigQuery 解法2
select
    store_cd
    , variance(amount) as variance
from `prj-test3.100knocks.receipt`
group by store_cd
order by variance desc
limit 5
;
```
Cf. [BigQueryで統計量を出す時に使うクエリメモ](https://qiita.com/hagino3000/items/e9ed62638ebe54391188) - Qiita

### | S-031 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の標本標準偏差を計算し、降順でTOP5を表示せよ。
```SQL
select
    store_cd
    , stddev(amount) as std
from `prj-test3.100knocks.receipt`
group by store_cd
order by std desc
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
from(
    select
        store_cd
        , percentile_cont(amount, 0.0) over(partition by store_cd) as min
        , percentile_cont(amount, 0.25) over(partition by store_cd) as q1
        , percentile_cont(amount, 0.5) over(partition by store_cd) as q2
        , percentile_cont(amount, 0.75) over(partition by store_cd) as q3
        , percentile_cont(amount, 1.0) over(partition by store_cd) as max
    from `prj-test3.100knocks.receipt`
)
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
from `prj-test3.100knocks.receipt`
group by store_cd
having mean > 330
;
```

### | S-034 ★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに売上金額 (amount)を合計して全顧客の平均を求めよ。ただし、顧客IDが"Z"から始まるのものは 非会員を表すため、除外して計算すること。
```SQL
-- select
--     customer_id
--     , sum(amount) as ttl_amount
-- from `prj-test3.100knocks.receipt`
-- where not regexp_contains(customer_id, r'^Z')
-- group by customer_id
-- ;

with
    customer_tb as(
        select
            customer_id
            , sum(amount) as ttl_amount
        from `prj-test3.100knocks.receipt`
        where not regexp_contains(customer_id, r'^Z')
        group by customer_id
    )
select avg(ttl_amount) from customer_tb;
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
        from `prj-test3.100knocks.receipt`
        where not regexp_contains(customer_id, r'^Z')
        group by customer_id
    )
select
    customer_id
    , ttl_amount
from customer_tb
where ttl_amount > (select avg(ttl_amount) from customer_tb)
limit 10
;
```

### | S-036 ★
レシート明細テーブル(receipt)と店舗テーブル(store)を内部結合し、レシート明細 テーブルの全項目と店舗テーブルの店舗名(store_name)を10件表示させよ。
```SQL
select
    *
from
    `prj-test3.100knocks.receipt`
left join
    (select store_cd, store_name from `prj-test3.100knocks.store`)
using(store_cd)
limit 10
;
```

### | S-037 ★
商品テーブル(product)とカテゴリテーブル(category)を内部結合し、商品テーブル の全項目とカテゴリテーブルの小区分名(category_small_name)を10件表示させよ。
```SQL
select
    *
from
    `prj-test3.100knocks.product` as p
left join
    (select category_major_cd, category_medium_cd, category_small_cd, category_small_name from `prj-test3.100knocks.category`) as c
on p.category_major_cd=c.category_major_cd
and p.category_medium_cd=c.category_medium_cd
and p.category_small_cd=c.category_small_cd
limit 10
;
```
```
select
    p.*
    , c.category_small_name
from `100knoks.product` as p
join `100knoks.category` as c
using(category_small_cd)
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
from `prj-test3.100knocks.customer` as c
join `prj-test3.100knocks.receipt` as r
using(customer_id)
where
    customer_id not like'Z%'
    and
    gender_cd = 1
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
from `prj-test3.100knocks.receipt`
where customer_id not like 'Z%'
group by customer_id
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
--     from `prj-test3.100knocks.store`)
--     +
--     (select
--         count(distinct product_cd)
--     from `prj-test3.100knocks.product`) as ttl_record

select
    count(*)
from `prj-test3.100knocks.store`
cross join `prj-test3.100knocks.product`
;
```
[cross join を知ると join が書きやすくなるよ、という話](https://developer.feedforce.jp/entry/2019/03/19/170000) -  Feedforce Developer Blog

### | S-041 ★★
レシート明細テーブル(receipt)の売上金額(amount)を日付(sales_ymd)ごとに集計し、前日からの売上金額増減を計算せよ。なお、計算結果は10件表示すればよい。
```SQL
with ttl_amount as (
    select
        sales_ymd
        , sum(amount) as amount
    from `prj-test3.100knocks.receipt`
    group by sales_ymd
    order by sales_ymd
)
select
    sales_ymd
    -- , amount
    -- , lag(amount) over(order by sales_ymd) as lag
    , lag(amount) over(order by sales_ymd) - amount as diff
from ttl_amount
order by sales_ymd
limit 10
;
```
```
%%sql
WITH sales_amount_by_date AS (
    SELECT sales_ymd, SUM(amount) as amount FROM receipt
    GROUP BY sales_ymd
    ORDER BY sales_ymd
)
SELECT sales_ymd, LAG(sales_ymd, 1) OVER(ORDER BY sales_ymd) lag_ymd,
    amount,
    LAG(amount, 1) OVER(ORDER BY sales_ymd) as lag_amount,
    amount - LAG(amount, 1) OVER(ORDER BY sales_ymd) as diff_amount
FROM sales_amount_by_date
LIMIT 10;
```
Cf.
*  [【BigQuery】LAG関数，LEAD関数の使い方
](https://qiita.com/kota_fujimura/items/cff732bb9acb47510a03) - Qiita
*  [【BIGQUERY】分析入門 - SECTION7](https://gitpress.io/c/google_bigquery/google_bigquery_7) - .tk

### | S-042 ★★
レシート明細テーブル(receipt)の売上金額(amount)を日付(sales_ymd)ごとに集 計し、各日付のデータに対し、1日前、2日前、3日前のデータを結合せよ。結果は10件 表示すればよい。
```SQL
with ttl_amount as (
    select
        sales_ymd
        , sum(amount) as amount
    from `prj-test3.100knocks.receipt`
    group by sales_ymd
    order by sales_ymd
)
select
    sales_ymd
    , amount
    , lag(amount, 1) over(order by sales_ymd) as amount_lag_1
    , lag(amount, 2) over(order by sales_ymd) as amount_lag_2
    , lag(amount, 3) over(order by sales_ymd) as amount_lag_3
from ttl_amount
order by sales_ymd
limit 10
;
```

### | S-043 ★
レシート明細テーブル(receipt)と顧客テーブル(customer)を結合し、性別 (gender)と年代(ageから計算)ごとに売上金額(amount)を合計した売上サマリテー ブル(sales_summary)を作成せよ。性別は0が男性、1が女性、9が不明を表すものとす る。ただし、項目構成は年代、女性の売上金額、男性の売上金額、性別不明の売上金額の4項 目とすること(縦に年代、横に性別のクロス集計)。また、年代は10歳ごとの階級とすること。
```SQL
with
    tb as (
        select
            sales_ymd
            , gender
            , age
            , case  
                when age < 20 then "10's"
                when age between 20 and 29 then "20's"
                when age between 30 and 39 then "30's"
                when age between 40 and 49 then "40's"
                when age between 50 and 59 then "50's"
                when age between 60 and 69 then "60's"
                when age between 70 and 79 then "70's"
                else "Over80's"
             end as age_category
            , amount
        from
            `prj-test3.100knocks.receipt`
        left join
            `prj-test3.100knocks.customer`
            using(customer_id)
        where gender is not null
    )
select
    *
from
    (select
        age_category
        , sum(amount) as male
    from tb
    where gender = "男性"
    group by age_category)
    left join
    (select
        age_category
        , sum(amount) as female
    from tb
    where gender = "女性"
    group by age_category) using(age_category)
    left join
    (select
        age_category
        , sum(amount) as unknown
    from tb
    where gender = "不明"
    group by age_category) using(age_category)
order by age_category
;
```
```
%%sql

-- SQL向きではないため、やや強引に記載する（カテゴリ数が多いときはとても長いSQLとなってしまう点に注意）

DROP TABLE IF EXISTS sales_summary;

CREATE TABLE sales_summary AS
    WITH gender_era_amount AS (
        SELECT c.gender_cd,
        TRUNC(age/ 10) * 10 AS era,
        SUM(r.amount) AS amount
        FROM customer c
        JOIN receipt r
        ON c.customer_id = r.customer_id
        GROUP BY c.gender_cd, era
    )
    select era,
        MAX(CASE gender_cd WHEN '0' THEN amount ELSE 0 END) AS male ,
        MAX(CASE gender_cd WHEN '1' THEN amount ELSE 0 END) AS female,
        MAX(CASE gender_cd WHEN '9' THEN amount ELSE 0 END) AS unknown
    FROM gender_era_amount
    GROUP BY era
    ORDER BY era
;
SELECT * FROM sales_summary;
```
[メモ](https://gist.github.com/GitHiru/ebee5fff3f44451cbcebfa7d408a160f) - Gist

### | S-044 ★
前設問で作成した売上サマリテーブル(sales_summary)は性別の売上を横持ちさせたも のであった。このテーブルから性別を縦持ちさせ、年代、性別コード、売上金額の3項目 に変換せよ。ただし、性別コードは男性を"00"、女性を"01"、不明を"99"とする。
```SQL
with
    tb as (
        select
            sales_ymd
            , gender
            , age
            , case  
                when age < 20 then "10's"
                when age between 20 and 29 then "20's"
                when age between 30 and 39 then "30's"
                when age between 40 and 49 then "40's"
                when age between 50 and 59 then "50's"
                when age between 60 and 69 then "60's"
                when age between 70 and 79 then "70's"
                else "Over80's"
             end as age_category
            , amount
        from
            `prj-test3.100knocks.receipt`
        left join
            `prj-test3.100knocks.customer`
            using(customer_id)
        where gender is not null
    )
select
    *
from
    (select
        '00' as gender_cd
        , age_category
        , sum(amount) as amount
    from tb
    where gender = "男性"
    group by age_category)
    union all
    (select
        '01' as gender_cd
        , age_category
        , sum(amount) as amount
    from tb
    where gender = "女性"
    group by age_category)
    union all
    (select
        '99' as gender_cd
        , age_category
        , sum(amount) as amount
    from tb
    where gender = "不明"
    group by age_category)
order by
    gender_cd
    , age_category
;
```

### | S-045 ★
顧客テーブル(customer)の生年月日(birth_day)は日付型でデータを保有している。 これをYYYYMMDD形式の文字列に変換し、顧客ID(customer_id)とともに抽出せよ。 データは10件を抽出すれば良い。
```SQL
select
    customer_id
    , replace(cast(birth_day as string), '-', '') as birth_day
from `prj-test3.100knocks.customer`
limit 10
;
```
Cf.[【SQL】空白やスペース、0、特定の文字を削除する方法](https://oreno-it.info/archives/2609) - SE日記

```
# PostgreSQL
%%sql
SELECT customer_id, TO_CHAR(birth_day, 'YYYYMMDD') FROM customer LIMIT 10;
```

### | S-046 ★
顧客テーブル(customer)の申し込み日(application_date)はYYYYMMDD形式の文字 列型でデータを保有している。これを日付型に変換し、顧客ID(customer_id)とともに 抽出せよ。データは10件を抽出すれば良い。
```SQL
select
    customer_id
    , parse_date('%Y%m%d', cast(application_date as string)) as application_date
from `prj-test3.100knocks.customer`
limit 10
;
```
Cf.
+ [BigQueryでstring型の文字列からdate型の日付に変換する](https://ten-ezo.com/62ac07f58f3d46f08e1eee524476acd8) - TEN's page
+ [[小ネタ] BigQueryで区切り文字のない年月日（YYYYMMDD）から日付型（YYYY-MM-DD）にする方法](https://dev.classmethod.jp/articles/omoitsuki-sonoichi/) - DevelopersIO

```
# PostgreSQL
%%sql
SELECT customer_id, TO_DATE(application_date, 'YYYYMMDD')
FROM customer LIMIT 10;
```

### | S-047 ★
レシート明細テーブル(receipt)の売上日(sales_ymd)はYYYYMMDD形式の数値型で データを保有している。これを日付型に変換し、レシート番号(receipt_no)、レシートサ ブ番号(receipt_sub_no)とともに抽出せよ。データは10件を抽出すれば良い。
```SQL
select
    receipt_no
    , receipt_sub_no
    , parse_date('%Y%m%d', cast(sales_ymd as string)) as sales_ymd
from `prj-test3.100knocks.receipt`
limit10
;
```

### | S-048 ★
レシート明細テーブル(receipt)の売上エポック秒(sales_epoch)は数値型のUNIX秒で データを保有している。これを日付型に変換し、レシート番号(receipt_no)、レシートサ ブ番号(receipt_sub_no)とともに抽出せよ。データは10件を抽出すれば良い。
```SQL
select
    receipt_no
    , receipt_sub_no
    , format_timestamp('%Y-%m-%d %H:%M:%S', timestamp_seconds(sales_epoch), 'Asia/Tokyo')as sales_epoch
from `prj-test3.100knocks.receipt`
limit10
;
```
Cf.
* [BigqueryでunixtimeをJSTに変換する](https://qiita.com/hogeta_/items/06ffeb48e8148adfda57) - Qiita
* [BigQuery関数 | unixtimeをDATETIMEに変換する](https://tokukichi.com/posts/444) - とくきちのゆるログ
* [TIMESTAMP_SECONDS](https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp_functions?hl=ja#timestamp_seconds) - GoogleCloud

```
# PostgreSQL
%%sql

SELECT
    TO_TIMESTAMP(sales_epoch) as sales_date,
    receipt_no, receipt_sub_no
FROM receipt
LIMIT 10;
```

### | S-049 ★
レシート明細テーブル(receipt)の売上エポック秒(sales_epoch)を日付型に変換し、 「年」だけ取り出してレシート番号(receipt_no)、レシートサブ番号(receipt_sub_no) とともに抽出せよ。データは10件を抽出すれば良い。
```SQL
select
    receipt_no
    , receipt_sub_no
    , format_date('%Y', cast(format_timestamp('%Y-%m-%d', timestamp_seconds(sales_epoch), 'Asia/Tokyo') as date)) as sales_epoch
from `prj-test3.100knocks.receipt`
limit10
;
```
Cf. [BIGQUERY】分析入門 - SECTION6](https://gitpress.io/c/google_bigquery/google_bigquery_6) - .tk

```
%%sql

SELECT
    TO_CHAR(EXTRACT(YEAR FROM TO_TIMESTAMP(sales_epoch)),'FM9999') as sales_year,
    receipt_no,
    receipt_sub_no
FROM receipt
LIMIT 10;
```

### | S-050 ★
レシート明細テーブル(receipt)の売上エポック秒(sales_epoch)を日付型に変換し、 「月」だけ取り出してレシート番号(receipt_no)、レシートサブ番号(receipt_sub_no) とともに抽出せよ。なお、「月」は0埋め2桁で取り出すこと。データは10件を抽出すれば 良い。
```SQL
select
    format_date('%m', cast(format_timestamp('%Y-%m-%d', timestamp_seconds(sales_epoch), 'Asia/Tokyo') as date)) as sales_epoch
    , receipt_no
    , receipt_sub_no
from `prj-test3.100knocks.receipt`
limit 10
;
```

### | S-051 ★
レシート明細テーブル(receipt)の売上エポック秒を日付型に変換し、「日」だけ取り出 してレシート番号(receipt_no)、レシートサブ番号(receipt_sub_no)とともに抽出せ よ。なお、「日」は0埋め2桁で取り出すこと。データは10件を抽出すれば良い。
```SQL
select
    format_date('%d', cast(format_timestamp('%Y-%m-%d', timestamp_seconds(sales_epoch), 'Asia/Tokyo') as date)) as sales_epoch
    , receipt_no
    , receipt_sub_no
from `prj-test3.100knocks.receipt`
limit 10
;
```

### | S-052 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計の上、売上金額合計に対して2,000円以下を0、2,000円より大きい金額を1に2値化し、顧客ID、売上金額合計とともに10件表示せよ。ただし、顧客IDが"Z"から始まるのも のは非会員を表すため、除外して計算すること。
```SQL
select
    customer_id
    , sum(amount) as amount
    , case
         when sum(amount) > 2000 then '1'
         else '0'
      end as label
from `prj-test3.100knocks.receipt`
where customer_id not like "Z%"
group by customer_id
limit 10
;
```

### | S-053 ★
顧客テーブル(customer)の郵便番号(postal_cd)に対し、東京(先頭3桁が100〜209 のもの)を1、それ以外のものを0に2値化せよ。さらにレシート明細テーブル(receipt) と結合し、全期間において売上実績がある顧客数を、作成した2値ごとにカウントせよ。
```SQL
with customer_classification as (
    select
        customer_id
        , postal_cd
        , case regexp_contains(postal_cd, r'^[1][0-9][0-9]-')
            when true then 1
            when false then 0
          end as label_100
        , case regexp_contains(postal_cd, r'^[2][0][0-9]-')
            when true then 1
            when false then 0
          end as label_200
    from `prj-test3.100knocks.customer`
)
select
    case
        when label_100 + label_200 = 0 then '0'
        else '1'
    end as label
    , count(customer_id)
from customer_classification
where
    customer_id in (select distinct customer_id from `prj-test3.100knocks.receipt`)
group by label
;
```
```
%%sql

WITH cust AS (
    SELECT
        customer_id,
        postal_cd,
        CASE
            WHEN 100 <= CAST(SUBSTR(postal_cd, 1, 3) AS INTEGER)
                    AND CAST(SUBSTR(postal_cd, 1, 3) AS INTEGER) <= 209 THEN 1
            ELSE 0
        END AS postal_flg
    FROM customer
),
rect AS(
    SELECT
        customer_id,
        SUM(amount)
    FROM
        receipt
    GROUP BY
        customer_id
)
SELECT
    c.postal_flg, count(1)
FROM
    rect r
JOIN
    cust c
ON
    r.customer_id = c.customer_id
GROUP BY
    c.postal_flg
```

### | S-054 ★
顧客テーブル(customer)の住所(address)は、埼玉県、千葉県、東京都、神奈川県の いずれかとなっている。都道府県毎にコード値を作成し、顧客ID、住所とともに抽出せよ。値は埼玉県を11、千葉県を12、東京都を13、神奈川県を14とすること。結果は10件 表示させれば良い。
```SQL
select
    customer_id
    , address
    , case
          when address like'%埼玉県%' then '11'
          when address like'%千葉県%' then '12'
          when address like'%東京都%' then '13'
          when address like'%神奈川県%' then '14'
      end as pref_label
from `prj-test3.100knocks.customer`
limit 10
;
```
```
%%sql

-- SQL向きではないため、やや強引に記載する（カテゴリ数が多いときはとても長いSQLとなってしまう点に注意）
SELECT
    customer_id,
    -- 確認用に住所も表示
    address,
    CASE SUBSTR(address,1, 3)
        WHEN '埼玉県' THEN '11'
        WHEN '千葉県' THEN '12'
        WHEN '東京都' THEN '13'
        WHEN '神奈川' THEN '14'
    END AS prefecture_cd
FROM
    customer
LIMIT 10
```

### | S-055 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、その合計金額の四分位点を求めよ。その上で、顧客ごとの売上金額合計に対して 以下の基準でカテゴリ値を作成し、顧客ID、売上金額合計とともに表示せよ。カテゴリ値 は順に1〜4とする。結果は10件表示させれば良い。
- 最小値以上第1四分位未満 ・・・ 1を付与
- 第1四分位以上第2四分位未満 ・・・ 2を付与
- 第2四分位以上第3四分位未満 ・・・ 3を付与
- 第3四分位以上 ・・・ 4を付与

```SQL
with
    customer_amount as (
        select
            customer_id
            , sum(amount) as amount
        from `prj-test3.100knocks.receipt`
        group by customer_id
    )
    , quatile as (
        select
            *
            , percentile_cont(amount, 0.0) over() as min
            , percentile_cont(amount, 0.25) over() as q1
            , percentile_cont(amount, 0.5) over() as q2
            , percentile_cont(amount, 0.75) over() as q3
            , percentile_cont(amount, 1.0) over() as max
        from customer_amount
    )
select
    customer_id
    , amount
    , case
          when amount < q1 then 1
          when amount >= q1 and amount < q2 then 2
          when amount >= q2 and amount < q3 then 3
          else 4
      end as label
from quatile
limit 10
;
```
```
%%sql

WITH sales_amount AS(
    SELECT
        customer_id,
        SUM(amount) as sum_amount
    FROM
        receipt
    GROUP BY
        customer_id
),
sales_pct AS (
    SELECT
        PERCENTILE_CONT(0.25) WITHIN GROUP(ORDER BY sum_amount) AS pct25,
        PERCENTILE_CONT(0.50) WITHIN GROUP(ORDER BY sum_amount) AS pct50,
        PERCENTILE_CONT(0.75) WITHIN GROUP(ORDER BY sum_amount) AS pct75
    FROM
        sales_amount
)
SELECT
    a.customer_id,
    a.sum_amount,
    CASE
        WHEN a.sum_amount < pct25 THEN 1
        WHEN pct25 <= a.sum_amount and a.sum_amount < pct50 THEN 2
        WHEN pct50 <= a.sum_amount and a.sum_amount < pct75 THEN 3
        WHEN pct75 <= a.sum_amount THEN 4
    END as pct_flg
FROM sales_amount a
CROSS JOIN sales_pct p
LIMIT 10
```

### | S-056 ★
顧客テーブル(customer)の年齢(age)をもとに10歳刻みで年代を算出し、顧客ID (customer_id)、生年月日(birth_day)とともに抽出せよ。ただし、60歳以上は全て60 歳代とすること。年代を表すカテゴリ名は任意とする。先頭10件を表示させればよい。
```SQL
select
    customer_id
    , birth_day
    , case trunc(age / 10) * 10
          when 10.0 then '10歳代'
          when 20.0 then '20歳代'
          when 30.0 then '30歳代'
          when 40.0 then '40歳代'
          when 50.0 then '50歳代'
          else '60歳代'
      end as era_label
from `prj-test3.100knocks.customer`
limit 10
;
```

### | S-057 ★
前問題の抽出結果と性別コード(gender_cd)により、新たに性別×年代の組み合わせを 表すカテゴリデータを作成せよ。組み合わせを表すカテゴリの値は任意とする。先頭10件 を表示させればよい。
```SQL
select
    customer_id
    , birth_day
    , concat(
          case gender_cd
              when 0 then '男性'
              when 1 then '女性'
              else '不明'
          end,
          '×',
          case trunc(age / 10) * 10
              when 10.0 then '10歳代'
              when 20.0 then '20歳代'
              when 30.0 then '30歳代'
              when 40.0 then '40歳代'
              when 50.0 then '50歳代'
              else '60歳代'
          end
      ) as gender_era_label
from `prj-test3.100knocks.customer`
limit 10
;
```

### | S-058 ★★
顧客テーブル(customer)の性別コード(gender_cd)を**ダミー変数化**し、顧客ID (customer_id)とともに抽出せよ。結果は10件表示させれば良い。
```SQL
select
     customer_id
     , case gender_cd when 0 then 1 else 0 end as male
     , case gender_cd when 1 then 1 else 0 end as female
     , case gender_cd when 9 then 1 else 0 end as unknown
from `prj-test3.100knocks.customer`
limit 10
;
```
CF[ダミー変数](https://octopus.nehan.io/manual/3375/) - nehan

### | S-059 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、売上金額合計を**平均0、標準偏差1に標準化**して顧客ID、売上金額合計とともに表 示せよ。標準化に使用する標準偏差は、不偏標準偏差と標本標準偏差のどちらでも良いも のとする。ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算す ること。結果は10件表示させれば良い。
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
     , standerd_scaler as (
          select
               *
               , avg(amount) over() as mean
               , stddev_pop(amount) over() as std
          from user_amount
     )
select
     customer_id
     , amount
     , (amount - mean)/std as ss_amount
from standerd_scaler
limit 10
;
```
Cf.[6-2. データを標準化してみよう](https://bellcurve.jp/statistics/course/19647.html) - 統計WEB

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
        AVG(sum_amount) as avg_amount,
        stddev_pop(sum_amount) as std_amount
    FROM
        sales_amount
)
SELECT
    customer_id,
    sum_amount,
    (sum_amount - avg_amount) / std_amount as normal_amount
FROM sales_amount
CROSS JOIN stats_amount
LIMIT 10
```

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
        -- 新単価式変換
        -- (p-c) / p  = 0.3
        -- p - c = 0.3p
        -- p = c/0.7 ∴
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

### | S-071 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、顧客テーブル (customer)の会員申込日(application_date)からの経過月数を計算し、顧客ID (customer_id)、売上日、会員申込日とともに表示せよ。結果は10件表示させれば良い (なお、sales_ymdは数値、application_dateは文字列でデータを保持している点に注意)。1ヶ月未満は切り捨てること。
```SQL
with tb as (
    select
        customer_id
        , parse_date('%Y%m%d', cast(sales_ymd as string)) as sales_ymd
        , parse_date('%Y%m%d', cast(application_date as string)) as application_date
        , date_diff(
            parse_date('%Y%m%d', cast(sales_ymd as string))
            , parse_date('%Y%m%d', cast(application_date as string))
            , month
          ) as month_diff
    from `prj-test3.100knocks.receipt`
        join `prj-test3.100knocks.customer`
            using(customer_id)
    )
select
    *
from tb
where month_diff > 1
limit 10
;
```

### | S-072 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、顧客テーブル (customer)の会員申込日(application_date)からの経過年数を計算し、顧客ID (customer_id)、売上日、会員申込日とともに表示せよ。結果は10件表示させれば良い (なお、sales_ymdは数値、application_dateは文字列でデータを保持している点に注 意)。1年未満は切り捨てること。
```SQL
with tb as (
    select
        customer_id
        , parse_date('%Y%m%d', cast(sales_ymd as string)) as sales_ymd
        , parse_date('%Y%m%d', cast(application_date as string)) as application_date
        , date_diff(
            parse_date('%Y%m%d', cast(sales_ymd as string))
            , parse_date('%Y%m%d', cast(application_date as string))
            , yaer
        ) as year_diff
    from `prj-test3.100knocks.receipt`
        join `prj-test3.100knocks.customer`
            using(customer_id)
    )
select
    *
from tb
where year_diff > 1
limit 10
;
```

### | S-073 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、顧客テーブル (customer)の会員申込日(application_date)からのエポック秒による経過時間を計算し、顧客ID(customer_id)、売上日、会員申込日とともに表示せよ。結果は10件表示さ せれば良い(なお、sales_ymdは数値、application_dateは文字列でデータを保持してい る点に注意)。なお、時間情報は保有していないため各日付は0時0分0秒を表すものとする。
```SQL
with tb as (
    select
        customer_id
        , sales_ymd
        , application_date
        , datetime(
            -- , format_date('%Y', parse_date('%Y%m%d', cast(sales_ymd as string)))
            cast(substr(cast(sales_ymd as string), 1, 4) as int64)
            , cast(substr(cast(sales_ymd as string), 5, 2) as int64)
            , cast(substr(cast(sales_ymd as string), 7, 2) as int64)
            , 0
            , 0
            , 0
        ) as sales_ymd_time
        , datetime(
            cast(substr(cast(application_date as string), 1, 4) as int64)
            , cast(substr(cast(application_date as string), 5, 2) as int64)
            , cast(substr(cast(application_date as string), 7, 2) as int64)
            , 0
            , 0
            , 0
        )as application_date_time
    from `prj-test3.100knocks.receipt`
        join `prj-test3.100knocks.customer`
            using(customer_id)
)
select
    customer_id
    , sales_ymd
    , application_date
    , datetime_diff(sales_ymd_time, application_date_time, second)
from tb
limit 10
;

```
Cf. [UNIX時間](https://ja.wikipedia.org/wiki/UNIX%E6%99%82%E9%96%93) - Wikipedia

### | S-074 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、当該週の月曜日からの経過日数を計算し、売上日、当該週の月曜日付とともに表示せよ。結果は10件表示させれば良い(なお、sales_ymdは数値でデータを保持している点に注意)。
```SQL
select
    customer_id
    -- , application_date
    , sales_ymd
    , date_trunc(parse_date('%Y%m%d', cast(sales_ymd as string)), week(monday)) as sales_ymd_on_monday
    , date_diff(
        date_trunc(parse_date('%Y%m%d', cast(sales_ymd as string)), week(monday))
        , parse_date('%Y%m%d', cast(sales_ymd as string))
        , day
    ) as date_diff
from `prj-test3.100knocks.receipt`
    join `prj-test3.100knocks.customer`
        using(customer_id)
limit 10
;
```
Cf.[[bigquery]対象日付を基準にして、特定曜日の日付を抽出する方法](https://apl-py.com/tec/bigquery%E5%AF%BE%E8%B1%A1%E6%97%A5%E4%BB%98%E3%82%92%E5%9F%BA%E6%BA%96%E3%81%AB%E3%81%97%E3%81%A6%E3%80%81%E7%89%B9%E5%AE%9A%E6%9B%9C%E6%97%A5%E3%81%AE%E6%97%A5%E4%BB%98%E3%82%92%E6%8A%BD%E5%87%BA) - 目黒で働く分析担当の作業メモ

### | S-075 ★
顧客テーブル(customer)からランダムに1%のデータを抽出し、先頭から10件データを抽出せよ。
```SQL
with
    tb as (
        select
            row_number() over() as rn
            , *
        from
            `prj-test3.100knocks.customer`
    )
    , random_tb as (
        select
            *
        from
            tb
        where
            mod(abs(farm_fingerprint(cast(rn as string))), 220) = 0
    )
select * from random_tb limit 10
;
```
Cf. [BigQuery でランダムサンプリング](https://medium.com/@sfujiwara/bigquery-%E3%81%A7%E3%83%A9%E3%83%B3%E3%83%80%E3%83%A0%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AA%E3%83%B3%E3%82%B0-807d64d1c2be) - medium

```
%%sql

-- コード例１（シンプルにやるなら）
SELECT * FROM customer WHERE RANDOM() <= 0.01
LIMIT 10
```
※レガシーSQLやPostgreSQL上記のように簡易的にかける。

### | S-076 ★★★
顧客テーブル(customer)から性別(gender_cd)の割合に基づきランダムに10%のデータを**層化抽出**し、性別ごとに件数を集計せよ。
```SQL
with
    tb as (
        select
            row_number() over() as rn
            , *
            , count(*) over(partition by gender_cd) as gender_cd_count
            , count(*) over() as ttl_count
            , (count(*) over(partition by gender_cd)) / (count(*) over()) as gender_ratio
        from
            `prj-test3.100knocks.customer`
        where
            gender_cd <> 9
    )
    /*********************************************
     * male=2,981 female=17,918 all=20,899
     * male=2,981/20,899  female=17,918/20,899
     * male=14            female=86
     * male:female=14:86
     *
     * male=2,981/14       female=17,918/86
     * male=212            female=208
     *********************************************/
    , stratified_sampling_gender_0 as (
        select
            *
        from tb
        where
            gender_cd = 0
            and
            mod(abs(farm_fingerprint(cast(rn as string))), 212) = 0
    )
    , stratified_sampling_gender_1 as (
        select
            *
        from tb
        where
            gender_cd = 1
            and
            mod(abs(farm_fingerprint(cast(rn as string))), 208) = 0
    )
select * from stratified_sampling_gender_1
union all
select * from stratified_sampling_gender_0
;
```
Cf.
* [16-3. 標本の抽出方法](https://bellcurve.jp/statistics/course/8007.html) - 統計WEB
* [BigQueryでブートストラップサンプリング](https://qiita.com/ognek/items/c590fa34ca7a59a2e284) - Qiita

### | S-077 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客単位に合計し、合計した売上金額の外れ値を抽出せよ。
ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算すること。なお、ここでは外れ値を平均から3σ以上離れたものとする。結果は10件表示させれば良い。
```SQL
with
    user_amount as (
        select
            customer_id
            , sum(amount) as amount
        from
            `prj-test3.100knocks.receipt`
        where
            customer_id not like 'Z%'
        group by
            customer_id
    )
    , statistics as (
        select
            *
            , round(avg(amount) over()) as mean
            , round(stddev(amount) over()) as siguma
            , round(stddev(amount) over())*3 as siguma_3
        from user_amount
    )
select
    customer_id
    , amount
from
    user_amount
where
    customer_id not in (
        select
            customer_id
        from
            statistics
        where
            amount between (mean-siguma) and (mean+siguma_3)
        )
limit 10
;
```

### | S-078 ★★
レシート明細テーブル(receipt)の売上金額(amount)を顧客単位に合計し、合計した売上金額の外れ値を抽出せよ。
ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算すること。
なお、ここでは外れ値を第一四分位と第三四分位の差であるIQRを用いて、「第一四分位数-1.5×IQR」よりも下回るもの、または「第三四分位数+1.5 ×IQR」を超えるものとする。
結果は10件表示させれば良い。
```SQL
with
    user_amount as (
        select
            customer_id
            , sum(amount) as amount
        from
            `prj-test3.100knocks.receipt`
        where
            customer_id not like 'Z%'
        group by
            customer_id
    )
    , statistics as (
        select
            *
            , round(avg(amount) over()) as mean
            , round(stddev(amount) over()) as siguma
            , min(amount)over() as min
            , percentile_cont(amount, 0.25) over() as q1
            , percentile_cont(amount, 0.5) over() as median
            , percentile_cont(amount, 0.75) over() as q3
            , max(amount)over() as max
            , trunc(percentile_cont(amount, 0.75) over() - percentile_cont(amount, 0.25) over()) as IQR
        from
            user_amount
    )
select
    customer_id
    , amount
from
    statistics
where
    amount < q1-1.5*IQR
    or
    amount > q3+1.5*IQR
limit 10
;
```
Cf.
>  [4-3. 外れ値検出のある箱ひげ図](https://bellcurve.jp/statistics/course/5222.html) - 統計WEB
>
> <img src='https://bellcurve.jp/statistics/wp-body/wp-content/uploads/2016/05/795316b92fc766b0181f6fef074f03fa-16.png' width=80%>

### | S-079 ★
商品テーブル(product)の各項目に対し、欠損数を確認せよ。
```SQL
with
    col_data as (
        select
            column_name as key
            , data_type
        from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
        where table_name='product'
    )
    , null_tb as (
        select 'product_cd' as key, count(*) as null_ct from `prj-test3.100knocks.product` where product_cd is null
        union all
        select 'category_major_cd' as key, count(*) as null_ct from `prj-test3.100knocks.product` where category_major_cd is null
        union all
        select 'category_medium_cd' as key, count(*) as null_ct from `prj-test3.100knocks.product` where category_medium_cd is null
        union all
        select 'category_small_cd' as key, count(*) as null_ct from `prj-test3.100knocks.product` where category_small_cd is null
        union all
        select 'unit_price' as key, count(*) as null_ct from `prj-test3.100knocks.product` where unit_price is null
        union all
        select 'unit_cost' as key, count(*) as null_ct from `prj-test3.100knoks.product` where unit_cost is null
    )
select
    *
from
    col_data  
join
    null_tb
using(key)
;
```

### | S-080 ★
商品テーブル(product)のいずれかの項目に欠損が発生しているレコードを全て削除した新たなproduct_1を作成せよ。なお、削除前後の件数を表示させ、前設問で確認した件数だけ減少していることも確認すること。
```SQL
with
    -- remove null inharit records.
    product_1 as (
        select
            *
        from
            `prj-test3.100knocks.product`
        where
            unit_cost is not null
    )
select
    '削除前' as key
    , count(*) as record
from `prj-test3.100knocks.product`
union all
select
    '削除後' as key
    , count(*) as record
from product_1
;
```

### | S-081 ★
単価(unit_price)と原価(unit_cost)の欠損値について、それぞれの平均値で補完した新たなproduct_2を作成せよ。なお、平均値については1円未満を丸めること(四捨五入または偶数への丸めで良い)。補完実施後、各項目について欠損が生じていないことも確認すること。
```SQL
with
    product_2 as (
        select
            product_cd
            , category_major_cd
            , category_medium_cd
            , category_small_cd
            , trunc(case unit_price is null
                when true then (select mean from(select avg(unit_price)over() as mean from `prj-test3.100knocks.product`)group by mean)
                else unit_price
            end) as unit_cost
            , trunc(case unit_cost is null
                when true then (select mean from(select avg(unit_cost)over() as mean from `prj-test3.100knocks.product`)group by mean)
                else unit_cost
            end) as unit_cost
        from
            `prj-test3.100knocks.product`
        -- where unit_price is null
    )
select * from product_2
;
```
Cf. [【BIGQUERY】分析入門 - SECTION6
](https://gitpress.io/c/google_bigquery/google_bigquery_6) - Gitpress

### | S-082 ★
単価(unit_price)と原価(unit_cost)の欠損値について、それぞれの中央値で補完した 新たなproduct_3を作成せよ。なお、中央値については1円未満を丸めること(四捨五入ま たは偶数への丸めで良い)。補完実施後、各項目について欠損が生じていないことも確認 すること。
```SQL
with
    product_3 as (
        select
            product_cd
            , category_major_cd
            , category_medium_cd
            , category_small_cd
            , trunc(case unit_price is null
                when true then (select q from(select percentile_cont(unit_price, 0.5)over() as q from `prj-test3.100knocks.product`)group by q)
                else unit_price
            end) as unit_price
            , trunc(case unit_cost is null
                when true then (select q from(select percentile_cont(unit_cost, 0.5)over() as q from `prj-test3.100knocks.product`)group by q)
                else unit_cost
            end) as unit_cost
        from
            `prj-test3.100knocks.product`
        -- where unit_price is null
    )
select * from product_3
;
```

### | S-083 ★★
単価(unit_price)と原価(unit_cost)の欠損値について、各商品の小区分 (category_small_cd)ごとに算出した中央値で補完した新たなproduct_4を作成せよ。
なお、中央値については1円未満を丸めること(四捨五入または偶数への丸めで良い)。
補完実施後、各項目について欠損が生じていないことも確認すること。
```SQL
with
    category_small_cd_gr as (
        select
            category_small_cd
            , sum(unit_price) as unit_price
            , sum(unit_cost) as unit_cost
        from
            `prj-test3.100knocks.product`
        group by
            category_small_cd
    )
    , product_4 as (
        select
            product_cd
            , category_major_cd
            , category_medium_cd
            , category_small_cd
            , trunc(
                case unit_price is null
                    when true then (select
                                        q
                                    from
                                        (select
                                            round(percentile_cont(unit_price, 0.5) over()) as q
                                         from
                                            category_small_cd_gr
                                         )
                                    group by
                                        q
                                    )
                    else unit_price
                end) as unit_price
            , trunc(
                case unit_cost is null
                    when true then (select
                                        q
                                    from
                                        (select
                                            round(percentile_cont(unit_cost, 0.5) over()) as q
                                         from
                                            category_small_cd_gr
                                         )
                                    group by
                                        q
                                    )
                    else unit_cost
                end) as unit_cost
        from
            `prj-test3.100knocks.product`
        -- where unit_price is null
    )
select * from product_4
;
```

### | S-084 ★★
顧客テーブル(customer)の全顧客に対し、全期間の売上金額に占める2019年売上金額の割合を計算せよ。
ただし、売上実績がない場合は0として扱うこと。
そして計算した割 合が0超のものを抽出せよ。
結果は10件表示させれば良い。
```SQL
with prep_tb as (
    select
        format_date('%Y', parse_date('%Y%m%d', cast(sales_ymd as string))) as sales_y
        , *
    from `prj-test3.100knocks.receipt`
        left join `prj-test3.100knocks.customer`
            using(customer_id)
)
-- (select sum(amount) as ttl_amount from prep_tb)
select
    sum(amount) as ttl_amount_2019
    , (select sum(amount) from prep_tb) as ttl_amount
    , (sum(amount) / (select sum(amount) from prep_tb))*100 as ratio
from
    prep_tb
where
    sales_y = '2019'
;

```

### | S-085 ★
顧客テーブル(customer)の全顧客に対し、郵便番号(postal_cd)を用いて経度緯度変 換用テーブル(geocode)を紐付け、新たなcustomer_1を作成せよ。ただし、複数紐づく 場合は経度(longitude)、緯度(latitude)それぞれ平均を算出すること。
```SQL
```

### | S-086 ★★★
前設問で作成した緯度経度つき顧客テーブル(customer_1)に対し、申込み店舗コード (application_store_cd)をキーに店舗テーブル(store)と結合せよ。そして申込み店舗 の緯度(latitude)・経度情報(longitude)と顧客の緯度・経度を用いて距離(km)を求 め、顧客ID(customer_id)、顧客住所(address)、店舗住所(address)とともに表示 せよ。計算式は簡易式で良いものとするが、その他精度の高い方式を利用したライブラリ を利用してもかまわない。結果は10件表示すれば良い。
```SQL
```

### | S-087 ★★
顧客テーブル(customer)では、異なる店舗での申込みなどにより同一顧客が複数登録さ れている。名前(customer_name)と郵便番号(postal_cd)が同じ顧客は同一顧客とみ なし、1顧客1レコードとなるように名寄せした名寄顧客テーブル(customer_u)を作成 せよ。ただし、同一顧客に対しては売上金額合計が最も高いものを残すものとし、売上金 額合計が同一もしくは売上実績がない顧客については顧客ID(customer_id)の番号が小 さいものを残すこととする。
```SQL
```

### | S-088 ★★
前設問で作成したデータを元に、顧客テーブルに統合名寄IDを付与したテーブル (customer_n)を作成せよ。ただし、統合名寄IDは以下の仕様で付与するものとする。
- 重複していない顧客:顧客ID(customer_id)を設定
- 重複している顧客:前設問で抽出したレコードの顧客IDを設定

```SQL
```

### | S-089 ★
売上実績がある顧客に対し、予測モデル構築のため学習用データとテスト用データに分割 したい。それぞれ8:2の割合でランダムにデータを分割せよ。
```SQL
```

### | S-090 ★★★
レシート明細テーブル(receipt)は2017年1月1日〜2019年10月31日までのデータを有し ている。売上金額(amount)を月次で集計し、学習用に12ヶ月、テスト用に6ヶ月のモデ ル構築用データを3テーブルとしてセット作成せよ。データの持ち方は自由とする。
```SQL
```

### | S-091 ★★★
顧客テーブル(customer)の各顧客に対し、売上実績がある顧客数と売上実績がない顧客 数が1:1となるようにアンダーサンプリングで抽出せよ。
```SQL
```

### | S-092 ★
顧客テーブル(customer)では、性別に関する情報が非正規化の状態で保持されている。 これを第三正規化せよ。
```SQL
```

### | S-093 ★
商品テーブル(product)では各カテゴリのコード値だけを保有し、カテゴリ名は保有し ていない。カテゴリテーブル(category)と組み合わせて非正規化し、カテゴリ名を保有 した新たな商品テーブルを作成せよ。
```SQL
```

### | S-094 ★
先に作成したカテゴリ名付き商品データを以下の仕様でファイル出力せよ。出力先のパス は"/tmp/data"を指定することでJupyterの"/work/data"と共有されるようになっている。 なお、COPYコマンドの権限は付与済みである。
- ファイル形式はCSV(カンマ区切り)
- ヘッダ有り
- 文字コードはUTF-8

```SQL
```

### | S-095 ★
先に作成したカテゴリ名付き商品データを以下の仕様でファイル出力せよ。出力先のパス は"/tmp/data"を指定することでJupyterの"/work/data"と共有されるようになっている。 なお、COPYコマンドの権限は付与済みである。
- ファイル形式はCSV(カンマ区切り)
- ヘッダ有り
- 文字コードはSJIS

```SQL
```

### | S-096 ★
先に作成したカテゴリ名付き商品データを以下の仕様でファイル出力せよ。出力先のパス は"/tmp/data"を指定することでJupyterの"/work/data"と共有されるようになっている。 なお、COPYコマンドの権限は付与済みである。
- ファイル形式はCSV(カンマ区切り)
- ヘッダ無し
- 文字コードはUTF-8

```SQL
```

### | S-097 ★
先に作成した以下形式のファイルを読み込み、テーブルを作成せよ。また、先頭3件を表 示させ、正しくとりまれていることを確認せよ。
- ファイル形式はCSV(カンマ区切り)
- ヘッダ有り
- 文字コードはUTF-8

```SQL
```

### | S-098 ★
先に作成した以下形式のファイルを読み込み、テーブルを作成せよ。また、先頭3件を表 示させ、正しくとりまれていることを確認せよ。
- ファイル形式はCSV(カンマ区切り)
- ヘッダ無し
- 文字コードはUTF-8

```SQL
```

### | S-099 ★
先に作成したカテゴリ名付き商品データを以下の仕様でファイル出力せよ。出力先のパス は"/tmp/data"を指定することでJupyterの"/work/data"と共有されるようになっている。 なお、COPYコマンドの権限は付与済みである。
- ファイル形式はTSV(タブ区切り)
- ヘッダ有り
- 文字コードはUTF-8

```SQL
```

### | S-100 ★
先に作成した以下形式のファイルを読み込み、テーブルを作成せよ。また、先頭10件を表 示させ、正しくとりまれていることを確認せよ。
- ファイル形式はTSV(タブ区切り)
- ヘッダ有り
- 文字コードはUTF-8

```SQL
```

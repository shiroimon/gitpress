---
date    : 2021-11-15
title   : 🔍 100本ノック
excerpt : 5１〜6０本ノック
tags    : ["🔍", "DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編
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

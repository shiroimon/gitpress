---
date    : 2021-11-15
title   : 41〜5０本ノック
excerpt : 
tags    : ["DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編
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

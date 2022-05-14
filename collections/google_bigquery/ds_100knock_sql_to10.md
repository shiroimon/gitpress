---
date    : 2021-11-15
title   : １〜１０本ノック
excerpt : 
tags    : ["DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編
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
from 
    `100knocks.receipt`
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
from 
    `100knocks.receipt`
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
from 
    `100knocks.receipt`
where   
    customer_id = 'CS018205000001'
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
from 
    `100knocks.receipt`
where
        customer_id = 'CS018205000001'
    and amount >= 1000
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
from 
    `100knocks.receipt`
where
        customer_id = 'CS018205000001'
    and (amount >= 1000 or quantity >= 5)
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
from 
    `100knocks.receipt`
where
        customer_id = 'CS018205000001'
    and amount between 1000 and 2000
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
from 
    `100knocks.receipt`
where
        customer_id = 'CS018205000001'
    and product_cd <> 'P071401019'
;
```

### | S-009 ★
以下の処理において、出力結果を変えずにORをANDに書き換えよ。

`select * from store where not (prefecture_cd = '13' or floor_area > 900)`

```sql
select
    *
from 
    `100knocks.store`
where
        prefecture_cd <> 13
    and floor_area < 900
;
```

### | S-010 ★
店舗テーブル(store)から、店舗コード(store_cd)が"S14"で始まるものだけ全項目抽 出し、10件だけ表示せよ。

```SQL
select
    *
from 
    `100knocks.store`
where 
    store_cd like 'S14%'
    -- where regexp_contains(store_cd, r'S14') #別解
limit 10
;
```

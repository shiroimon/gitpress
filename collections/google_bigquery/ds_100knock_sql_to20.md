---
date    : 2021-11-15
title   : 🔍 100本ノック
excerpt : １１〜2０本ノック
tags    : ["🔍 ", "DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編
### | S-011 ★
顧客テーブル(customer)から顧客ID(customer_id)の末尾が1のものだけ全項目抽出 し、10件だけ表示せよ。

```SQL
select
    *
from 
   `100knocks.customer`
where 
   regexp_contains(customer_id, r'1$')
limit 10
;
```

### | S-012 ★
店舗テーブル(store)から横浜市の店舗だけ全項目表示せよ。
```SQL
select
    *
from 
   `100knocks.store`
where 
   address like '%横浜市%'
;
```

### | S-013 ★★
顧客テーブル(customer)から、ステータスコード(status_cd)の先頭がアルファベッ トのA〜Fで始まるデータを全項目抽出し、10件だけ表示せよ。
```SQL
select
    *
from
   `100knocks.customer`
where
    regexp_contains(customer_id, r'^(A|B|C|D|E|F)')
    -- regexp_contains(customer_id, r'^(A-F)') -- 別解
    --    customer_id like 'A%'
    -- or customer_id like 'B%'
    -- or customer_id like 'C%'
    -- or customer_id like 'D%'
    -- or customer_id like 'E%'
    -- or customer_id like 'F%' -- 別解
limit 10
;
```
Cf. [BigQueryでLIKE文の複数条件指定をORから正規表現に直す](https://qiita.com/mida12251141/items/27fe42ef01e83d5b063f) - Qiita

### | S-014 ★★
顧客テーブル(customer)から、ステータスコード(status_cd)の末尾が数字の1〜9で 終わるデータを全項目抽出し、10件だけ表示せよ。
```SQL
select
    *
from 
   `100knocks.customer`
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
from 
   `100knocks.customer`
where
    regexp_contains(status_cd, r'^(A-F)-\d+-[1-9]$')
limit 10
;
```
Cf. [基本的な正規表現一覧](https://murashun.jp/article/programming/regular-expression.html) - murashun.jp

### | S-016 ★★
店舗テーブル(store)から、電話番号(tel_no)が3桁-3桁-4桁のデータを全項目表示せよ。
```SQL
select
    *
from 
    `100knocks.store`
where 
    regexp_contains(tel_no, r'\d{3}-\d{3}-\d{4}')
;
```

### | S-017 ★
顧客テーブル(customer)を生年月日(birth_day)で高齢順にソートし、先頭10件を全項目表示せよ。
```SQL
select
    *
from 
   `100knocks.customer`
order by 
   birth_day
limit 10
;
```

### | S-018 ★
顧客テーブル(customer)を生年月日(birth_day)で若い順にソートし、先頭10件を全 項目表示せよ。
```SQL
select
    *
from 
   `100knocks.customer`
order by 
   birth_day desc
limit 10
;
```

### | S-019 ★★
レシート明細テーブル(receipt)に対し、1件あたりの売上金額(amount)が高い順にラ ンクを付与し、先頭10件を抽出せよ。項目は顧客ID(customer_id)、売上金額 (amount)、付与したランクを表示させること。なお、売上金額(amount)が等しい場合は同一順位を付与するものとする。
```SQL
select
    customer_id
    , amount
    , rank() over (order by amount desc) as ranking
from 
   `100knocks.receipt`
order by 
   ranking
limit 10
;
```

### | S-020 ★★
レシート明細テーブル(receipt)に対し、1件あたりの売上金額(amount)が高い順にラ ンクを付与し、先頭10件を抽出せよ。項目は顧客ID(customer_id)、売上金額 (amount)、付与したランクを表示させること。なお、売上金額(amount)が等しい場 合でも別順位を付与すること。
```SQL
select
    customer_id
    , amount
    , dense_rank()　over (order by amount desc) as ranking -- row_number()別解
from 
   `100knocks.receipt`
order by 
   ranking
limit 10
;
```

Cf. [【BIGQUERY】分析入門 - SECTION7](https://gitpress.io/c/google_bigquery/google_bigquery_7)- .tk

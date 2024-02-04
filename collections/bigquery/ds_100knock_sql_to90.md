---
date    : 2021-11-15
title   : 🔍 100本ノック
excerpt : 8１〜9０本ノック
tags    : ["DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編
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
            , trunc(
                case unit_price is null
                    when true then (select mean from (select avg(unit_price) over () as ttl_mean from `prj-test3.100knocks.product`) group by mean)
                    else unit_price
                end) as unit_cost
            , trunc(
                case unit_cost is null
                    when true then (select mean from (select avg(unit_cost) over () as ttl_mean from `prj-test3.100knocks.product`) group by mean)
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
            , trunc(
                case unit_price is null
                    when true then (select q from(select percentile_cont(unit_price, 0.5) over () as q from `prj-test3.100knocks.product`) group by q)
                   else unit_price
                end) as unit_price
            , trunc(
                case unit_cost is null
                    when true then (select q from(select percentile_cont(unit_cost, 0.5) over () as q from `prj-test3.100knocks.product`) group by q)
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
                    when true 
                    then (select q
                          from (select round(percentile_cont(unit_price, 0.5) over ()) as q from category_small_cd_gr)
                          group by q )
                    else unit_price
                end) as unit_price
            , trunc(
                case unit_cost is null
                    when true 
                    then (select q
                          from (select round(percentile_cont(unit_cost, 0.5) over ()) as q from category_small_cd_gr)
                          group by q )
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
    from 
        `prj-test3.100knocks.receipt`
        left join `prj-test3.100knocks.customer` using(customer_id)
)
-- (select sum(amount) as ttl_amount from prep_tb)
select
    sum(amount) as ttl_amount_2019
    , (select sum(amount) from prep_tb) as ttl_amount
    , (sum(amount) / (select sum(amount) from prep_tb)) * 100 as ratio
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

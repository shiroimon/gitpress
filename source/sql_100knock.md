---
date    : 2021-11-15
title   : 【🔎 SQL】データサイエンス100本ノック（構造化データ加工編）
excerpt : データサイエンス協会が、データサイエンス初学者のための実践的な学習環境をGitHubに無料公開！
tags    : ["DataScientist", "SQL", "BigQuery"]
---
## SQL編

### | EDA
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
レシート明細のテーブル(receipt)から売上日(sales_ymd)、顧客ID (customer_id)、商品コード(product_cd)、売上金額(amount)の順に列を指定し、 以下の条件を満たすデータを抽出せよ。
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
    regexp_contains(customer_id, r'^(A|B|C|D|E|F)')
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

```SQL
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
```SQL
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


```SQL
# PostgreSQL
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
```

### | S-030 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の標本分散を計算し、降順でTOP5を表示せよ。
```SQL
```

### | S-031 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の標本標準偏差を計算し、降順でTOP5を表示せよ。
```SQL
```

### | S-032 ★
レシート明細テーブル(receipt)の売上金額(amount)について、25%刻みでパーセン タイル値を求めよ。
```SQL
```

### | S-033 ★
レシート明細テーブル(receipt)に対し、店舗コード(store_cd)ごとに売上金額 (amount)の平均を計算し、330以上のものを抽出せよ。
```SQL
```

### | S-034 ★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに売上金額 (amount)を合計して全顧客の平均を求めよ。ただし、顧客IDが"Z"から始まるのものは 非会員を表すため、除外して計算すること。
```SQL
```

### | S-035 ★★
レシート明細テーブル(receipt)に対し、顧客ID(customer_id)ごとに売上金額 (amount)を合計して全顧客の平均を求め、平均以上に買い物をしている顧客を抽出せよ。ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算すること。なお、データは10件だけ表示させれば良い。
```SQL
```

### | S-036 ★
レシート明細テーブル(receipt)と店舗テーブル(store)を内部結合し、レシート明細 テーブルの全項目と店舗テーブルの店舗名(store_name)を10件表示させよ。
```SQL
```

### | S-037 ★
商品テーブル(product)とカテゴリテーブル(category)を内部結合し、商品テーブル の全項目とカテゴリテーブルの小区分名(category_small_name)を10件表示させよ。
```SQL
```

### | S-038 ★
顧客テーブル(customer)とレシート明細テーブル(receipt)から、各顧客ごとの売上 金額合計を求めよ。ただし、売上実績がない顧客については売上金額を0として表示させ ること。また、顧客は性別コード(gender_cd)が女性(1)であるものを対象とし、非会員(顧客IDが"Z"から始まるもの)は除外すること。なお、結果は10件だけ表示させれば 良い。
```SQL
```

### | S-039 ★
レシート明細テーブル(receipt)から売上日数の多い顧客の上位20件と、売上金額合計の多い顧客の上位20件を抽出し、完全外部結合せよ。ただし、非会員(顧客IDが"Z"から始まるもの)は除外すること。
```SQL
```

### | S-040 ★
全ての店舗と全ての商品を組み合わせると何件のデータとなるか調査したい。店舗 (store)と商品(product)を直積した件数を計算せよ。
```SQL
```

### | S-041 ★★
レシート明細テーブル(receipt)の売上金額(amount)を日付(sales_ymd)ごとに集計し、前日からの売上金額増減を計算せよ。なお、計算結果は10件表示すればよい。
```SQL
```

### | S-042 ★★
レシート明細テーブル(receipt)の売上金額(amount)を日付(sales_ymd)ごとに集 計し、各日付のデータに対し、1日前、2日前、3日前のデータを結合せよ。結果は10件 表示すればよい。
```SQL
```

### | S-043 ★
レシート明細テーブル(receipt)と顧客テーブル(customer)を結合し、性別 (gender)と年代(ageから計算)ごとに売上金額(amount)を合計した売上サマリテー ブル(sales_summary)を作成せよ。性別は0が男性、1が女性、9が不明を表すものとす る。
ただし、項目構成は年代、女性の売上金額、男性の売上金額、性別不明の売上金額の4項 目とすること(縦に年代、横に性別のクロス集計)。また、年代は10歳ごとの階級とすること。
```SQL
```

### | S-044 ★
前設問で作成した売上サマリテーブル(sales_summary)は性別の売上を横持ちさせたも のであった。このテーブルから性別を縦持ちさせ、年代、性別コード、売上金額の3項目 に変換せよ。ただし、性別コードは男性を"00"、女性を"01"、不明を"99"とする。
```SQL
```

### | S-045 ★
顧客テーブル(customer)の生年月日(birth_day)は日付型でデータを保有している。 これをYYYYMMDD形式の文字列に変換し、顧客ID(customer_id)とともに抽出せよ。 データは10件を抽出すれば良い。
```SQL
```

### | S-046 ★
顧客テーブル(customer)の申し込み日(application_date)はYYYYMMDD形式の文字 列型でデータを保有している。これを日付型に変換し、顧客ID(customer_id)とともに 抽出せよ。データは10件を抽出すれば良い。
```SQL
```

### | S-047 ★
レシート明細テーブル(receipt)の売上日(sales_ymd)はYYYYMMDD形式の数値型で データを保有している。これを日付型に変換し、レシート番号(receipt_no)、レシートサ ブ番号(receipt_sub_no)とともに抽出せよ。データは10件を抽出すれば良い。
```SQL
```

### | S-048 ★
レシート明細テーブル(receipt)の売上エポック秒(sales_epoch)は数値型のUNIX秒で データを保有している。これを日付型に変換し、レシート番号(receipt_no)、レシートサ ブ番号(receipt_sub_no)とともに抽出せよ。データは10件を抽出すれば良い。
```SQL
```

### | S-049 ★
レシート明細テーブル(receipt)の売上エポック秒(sales_epoch)を日付型に変換し、 「年」だけ取り出してレシート番号(receipt_no)、レシートサブ番号(receipt_sub_no) とともに抽出せよ。データは10件を抽出すれば良い。
```SQL
```

### | S-050 ★
レシート明細テーブル(receipt)の売上エポック秒(sales_epoch)を日付型に変換し、 「月」だけ取り出してレシート番号(receipt_no)、レシートサブ番号(receipt_sub_no) とともに抽出せよ。なお、「月」は0埋め2桁で取り出すこと。データは10件を抽出すれば 良い。
```SQL
```

### | S-051 ★
レシート明細テーブル(receipt)の売上エポック秒を日付型に変換し、「日」だけ取り出 してレシート番号(receipt_no)、レシートサブ番号(receipt_sub_no)とともに抽出せ よ。なお、「日」は0埋め2桁で取り出すこと。データは10件を抽出すれば良い。
```SQL
```

### | S-052 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計の上、売上金額合計に対して2,000円以下を0、2,000円より大きい金額を1に2値化 し、顧客ID、売上金額合計とともに10件表示せよ。ただし、顧客IDが"Z"から始まるのも のは非会員を表すため、除外して計算すること。
```SQL
```

### | S-053 ★
顧客テーブル(customer)の郵便番号(postal_cd)に対し、東京(先頭3桁が100〜209 のもの)を1、それ以外のものを0に2値化せよ。さらにレシート明細テーブル(receipt) と結合し、全期間において売上実績がある顧客数を、作成した2値ごとにカウントせよ。
```SQL
```

### | S-054 ★
顧客テーブル(customer)の住所(address)は、埼玉県、千葉県、東京都、神奈川県の いずれかとなっている。都道府県毎にコード値を作成し、顧客ID、住所とともに抽出せ よ。値は埼玉県を11、千葉県を12、東京都を13、神奈川県を14とすること。結果は10件 表示させれば良い。
```SQL
```

### | S-055 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、その合計金額の四分位点を求めよ。その上で、顧客ごとの売上金額合計に対して 以下の基準でカテゴリ値を作成し、顧客ID、売上金額合計とともに表示せよ。カテゴリ値 は順に1〜4とする。結果は10件表示させれば良い。
- 最小値以上第1四分位未満 ・・・ 1を付与
- 第1四分位以上第2四分位未満 ・・・ 2を付与
- 第2四分位以上第3四分位未満 ・・・ 3を付与
- 第3四分位以上 ・・・ 4を付与

```SQL
```

### | S-056 ★
顧客テーブル(customer)の年齢(age)をもとに10歳刻みで年代を算出し、顧客ID (customer_id)、生年月日(birth_day)とともに抽出せよ。ただし、60歳以上は全て60 歳代とすること。年代を表すカテゴリ名は任意とする。先頭10件を表示させればよい。
```SQL
```

### | S-057 ★
前問題の抽出結果と性別コード(gender_cd)により、新たに性別×年代の組み合わせを 表すカテゴリデータを作成せよ。組み合わせを表すカテゴリの値は任意とする。先頭10件 を表示させればよい。
```SQL
```

### | S-058 ★★
顧客テーブル(customer)の性別コード(gender_cd)をダミー変数化し、顧客ID (customer_id)とともに抽出せよ。結果は10件表示させれば良い。
```SQL
```

### | S-059 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、売上金額合計を平均0、標準偏差1に標準化して顧客ID、売上金額合計とともに表 示せよ。標準化に使用する標準偏差は、不偏標準偏差と標本標準偏差のどちらでも良いも のとする。ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算す ること。結果は10件表示させれば良い。
```SQL
```

### | S-060 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、売上金額合計を最小値0、最大値1に正規化して顧客ID、売上金額合計とともに表 示せよ。ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算する こと。結果は10件表示させれば良い。
```SQL
```

### | S-061 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、売上金額合計を常用対数化(底=10)して顧客ID、売上金額合計とともに表示せ よ。ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算するこ と。結果は10件表示させれば良い。
```SQL
```

### | S-062 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客ID(customer_id)ごとに 合計し、売上金額合計を自然対数化(底=e)して顧客ID、売上金額合計とともに表示せよ (ただし、顧客IDが"Z"から始まるのものは非会員を表すため、除外して計算するこ と)。結果は10件表示させれば良い。
```SQL
```

### | S-063 ★
商品テーブル(product)の単価(unit_price)と原価(unit_cost)から、各商品の利益額 を算出せよ。結果は10件表示させれば良い。
```SQL
```

### | S-064 ★
商品テーブル(product)の単価(unit_price)と原価(unit_cost)から、各商品の利益率 の全体平均を算出せよ。 ただし、単価と原価にはNULLが存在することに注意せよ。
```SQL
```

### | S-065 ★
商品テーブル(product)の各商品について、利益率が30%となる新たな単価を求めよ。 ただし、1円未満は切り捨てること。そして結果を10件表示させ、利益率がおよそ30%付 近であることを確認せよ。ただし、単価(unit_price)と原価(unit_cost)にはNULLが 存在することに注意せよ。
```SQL
```

### | S-066 ★
商品テーブル(product)の各商品について、利益率が30%となる新たな単価を求めよ。 今回は、1円未満を丸めること(四捨五入または偶数への丸めで良い)。そして結果を10 件表示させ、利益率がおよそ30%付近であることを確認せよ。ただし、単価 (unit_price)と原価(unit_cost)にはNULLが存在することに注意せよ。
```SQL
```

### | S-067 ★
商品テーブル(product)の各商品について、利益率が30%となる新たな単価を求めよ。 今回は、1円未満を切り上げること。そして結果を10件表示させ、利益率がおよそ30%付 近であることを確認せよ。ただし、単価(unit_price)と原価(unit_cost)にはNULLが 存在することに注意せよ。
```SQL
```

### | S-068 ★
商品テーブル(product)の各商品について、消費税率10%の税込み金額を求めよ。 1円未 満の端数は切り捨てとし、結果は10件表示すれば良い。ただし、単価(unit_price)には NULLが存在することに注意せよ。
```SQL
```

### | S-069 ★★
レシート明細テーブル(receipt)と商品テーブル(product)を結合し、顧客毎に全商品 の売上金額合計と、カテゴリ大区分(category_major_cd)が"07"(瓶詰缶詰)の売上金 額合計を計算の上、両者の比率を求めよ。抽出対象はカテゴリ大区分"07"(瓶詰缶詰)の 売上実績がある顧客のみとし、結果は10件表示させればよい。
```SQL
```

### | S-070 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、顧客テーブル (customer)の会員申込日(application_date)からの経過日数を計算し、顧客ID (customer_id)、売上日、会員申込日とともに表示せよ。結果は10件表示させれば良い (なお、sales_ymdは数値、application_dateは文字列でデータを保持している点に注 意)。
```SQL
```

### | S-071 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、顧客テーブル (customer)の会員申込日(application_date)からの経過月数を計算し、顧客ID (customer_id)、売上日、会員申込日とともに表示せよ。結果は10件表示させれば良い (なお、sales_ymdは数値、application_dateは文字列でデータを保持している点に注 意)。1ヶ月未満は切り捨てること。
```SQL
```

### | S-072 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、顧客テーブル (customer)の会員申込日(application_date)からの経過年数を計算し、顧客ID (customer_id)、売上日、会員申込日とともに表示せよ。結果は10件表示させれば良い (なお、sales_ymdは数値、application_dateは文字列でデータを保持している点に注 意)。1年未満は切り捨てること。
```SQL
```

### | S-073 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、顧客テーブル (customer)の会員申込日(application_date)からのエポック秒による経過時間を計算 し、顧客ID(customer_id)、売上日、会員申込日とともに表示せよ。結果は10件表示さ せれば良い(なお、sales_ymdは数値、application_dateは文字列でデータを保持してい る点に注意)。なお、時間情報は保有していないため各日付は0時0分0秒を表すものとす る。
```SQL
```

### | S-074 ★★
レシート明細テーブル(receipt)の売上日(sales_ymd)に対し、当該週の月曜日からの 経過日数を計算し、売上日、当該週の月曜日付とともに表示せよ。結果は10件表示させれ ば良い(なお、sales_ymdは数値でデータを保持している点に注意)。
```SQL
```

### | S-075 ★
顧客テーブル(customer)からランダムに1%のデータを抽出し、先頭から10件データを 抽出せよ。
```SQL
```

### | S-076 ★★★
顧客テーブル(customer)から性別(gender_cd)の割合に基づきランダムに10%のデー タを層化抽出し、性別ごとに件数を集計せよ。
```SQL
```

### | S-077 ★
レシート明細テーブル(receipt)の売上金額(amount)を顧客単位に合計し、合計した 売上金額の外れ値を抽出せよ。ただし、顧客IDが"Z"から始まるのものは非会員を表すた め、除外して計算すること。なお、ここでは外れ値を平均から3σ以上離れたものとす る。結果は10件表示させれば良い。
```SQL
```

### | S-078 ★★
レシート明細テーブル(receipt)の売上金額(amount)を顧客単位に合計し、合計した 売上金額の外れ値を抽出せよ。ただし、顧客IDが"Z"から始まるのものは非会員を表すた め、除外して計算すること。なお、ここでは外れ値を第一四分位と第三四分位の差である IQRを用いて、「第一四分位数-1.5×IQR」よりも下回るもの、または「第三四分位数+1.5 ×IQR」を超えるものとする。結果は10件表示させれば良い。
```SQL
```

### | S-079 ★
商品テーブル(product)の各項目に対し、欠損数を確認せよ。
```SQL
```

### | S-080 ★
商品テーブル(product)のいずれかの項目に欠損が発生しているレコードを全て削除し た新たなproduct_1を作成せよ。なお、削除前後の件数を表示させ、前設問で確認した件 数だけ減少していることも確認すること。
```SQL
```

### | S-081 ★
単価(unit_price)と原価(unit_cost)の欠損値について、それぞれの平均値で補完した 新たなproduct_2を作成せよ。なお、平均値については1円未満を丸めること(四捨五入ま たは偶数への丸めで良い)。補完実施後、各項目について欠損が生じていないことも確認 すること。
```SQL
```

### | S-082 ★
単価(unit_price)と原価(unit_cost)の欠損値について、それぞれの中央値で補完した 新たなproduct_3を作成せよ。なお、中央値については1円未満を丸めること(四捨五入ま たは偶数への丸めで良い)。補完実施後、各項目について欠損が生じていないことも確認 すること。
```SQL
```

### | S-083 ★★
単価(unit_price)と原価(unit_cost)の欠損値について、各商品の小区分 (category_small_cd)ごとに算出した中央値で補完した新たなproduct_4を作成せよ。な お、中央値については1円未満を丸めること(四捨五入または偶数への丸めで良い)。補 完実施後、各項目について欠損が生じていないことも確認すること。
```SQL
```

### | S-084 ★★
顧客テーブル(customer)の全顧客に対し、全期間の売上金額に占める2019年売上金額 の割合を計算せよ。ただし、売上実績がない場合は0として扱うこと。そして計算した割 合が0超のものを抽出せよ。 結果は10件表示させれば良い。
```SQL
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

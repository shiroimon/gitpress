---
date   : 2021-09-10
title  : 【BigQuery】分析入門 - Section4
excerpt: Google BigQuery基本の「き」について。
tags   : ["Google BigQuery", "SQL基本", "分析基本"]
---

## || Section4

### | SELECT句 - かラム指定
ex.【4.2 演習問題1(1:00)】

shop_purchasesテーブルからpurchase_id,user_id,dateを取り出してください。
```SQL
SELECT purchase_id, user_id, date FROM bq_sample.shop_purchases;
```

### | DISTINCT句 - 重複除外
ex.【4.2 演習問題2(3:45)】

shop_purchasesを利用して、固有のユーザーが何人いるかを取得。
```SQL
SELECT DISTINCT user_id FROM bq_sample.shop_purchases;
```

### | EXCEPT() - かラム除外
ex.【4.3 演習問題1(1:50)】

shop_purchasesテーブルから全カラムを取得。
```SQL
SELECT * FROM bq_sample.shop_purchases; -- (70.1KB)
```
ex.【4.3 演習問題2(3:20)】

shop_purchasesテーブルからshop_id、product_idを除くカラムを取得。
```SQL
SELECT
    * EXCEPT(shop_id, product_id)
FROM bq_sample.shop_purchases; --(50.1KB)
```

### | ORDERBY句 - ソート機能
e.g.
```
ORDER BY {カラム番号,カラム} (ASC:昇順 DESC:降順） … [1]indexに注意
```
ex.【4.4 演習問題1(2:31)】

shop_purchasesテーブルから、user_id、quantityを取得し、quantityの大き異順に並び替えてください。
```SQL
SELECT
    user_id,
    quantity
FROM bq_sample.shop_purchases
ORDER BY quantity DESC; --ORDER BY 2 DESC;
```

ex.【4.4 演習問題2(5:06)】

shop_purchasesテーブルンの全列を取得し、dateの古い順に並べてください。
もし、同じ日に複数行ある場合、sales_amountの大きい順に並べてください。
sales_amountも同額であった場合には、quantityの大きい順に並べてください。
```SQL
SELECT *
FROM bq_sample.shop_purchases
ORDER BY date, sales_amount DESC, quantity desc;
--日付順(昇順) ＋ 同日の場合売上高降順 + 数量降順
```

ex.【4.4 演習問題3(8:20)】

shop_purchasesテーブルの全列を取得し、dateの新しい順に並べてください。
もし同じ日に複数のレコードがある場合には、sales_amountの小さい順に並べてください。
但、並べ替えには列の順序を利用すること。
```SQL
SELECT *
FROM bq_sample.shop_purchases
ORDER BY 3 desc, 7;
-- ORDER BY data desc, sales_amount;
```

### | LIMIT句 - レコード制限
```
※ 表示レコード数を制限するだけで、後ろの計算は全行行われている。
   そのため発生料金に対して節約できるわけではない。
```
ex.【4.5 演習問題1 (0:55)】

shop_purchasesテーブルから、全カラムを5行だけ取得してください。
```SQL
SELECT *
FROM bq_sample.shop_purchases
LIMIT 5;
```
ex.【4.5 演習問題2 (2:23)】

shop_purchasesテーブルから、date大きい順（＝新しい順）に、全カラムを10行だけ取得してください。
```SQL
SELECT *
FROM `prj-test3.bq_sample.shop_purchases`
ORDER BY date DESC
LIMIT 10;
```

#### OFFSET
e.g.
```
LIMIT [取得したい行数] OFFSET [取得をずらす行数];
※ ORDERBY を使用前提
```
ex.【4.5 演習問題3 (:)】取得制限

shop_purchasesテーブルから、purchase_id、sales_amountを取得。その際sales_amont降順に並べた上で11位から5レコードだけを取得。
```SQL
SELECT purchase_id, sales_amount
FROM `prj-test3.bq_sample.shop_purchases`
ORDER BY sales_amount DESC
LIMIT 5 OFFSET 11;
```

### | WHERE句 - 絞り込み条件
e.g.
```SQL
SELECT * FROM bq_sample.shop_purchases WHERE quantity > 3;
```

#### ■ 数字型
##### | 比較演算子
e.g.
```SQL
-- 売上高¥10,000以上のレコード
WHERE sales-amount > 10000
-- 数量3以下のレコード
WHERE quantity <= 3
```
|||
|-|-|
|=||
|!= , <>|否定|
|> , >=|以上、より|
|< , <=|以下、未満|

##### | BETWEEN句
e.g.
```SQL
-- ¥8,500~¥9,800に該当レコード
WHERE list_price BETWEEN 8500 and 9800
-- ¥8,500~¥9,800に該当レコード以外
WHERE list_price NOT BETWEEN 8500 and 9800
```
##### | IN句
e.g.
```SQL
-- 数量が「3」「7」「11」にに該当レコード
WHERE quantity IN(3, 7, 11)
-- 数量が「3」「7」「11」にに該当レコード以外
WHERE quantity NOT IN(3, 7, 11)
```

ex.【4.7 演習問題1(:)】

```SQL
SELECT user_id, quantity
FROM bq_sample.shop_purchases
WHERE quantity > 3
ORDER BY 1;
```
ex.【4.7 演習問題2(7:15)】

```SQL
SELECT user_id, sales_amount
FROM bq_sample.shop_purchases
WHERE sales_amount BETWEEN 5000 AND 10000;
```
ex. 【4.7 演習問題3(8:15)】

```SQL
SELECT user_id, date, sales_amount, shop_id
FROM `prj-test3.bq_sample.shop_purchases`
WHERE shop_id IN(1, 3, 4);
```

#### ■ 文字型
e.g.
```SQL
--名前が鈴木のレコード
WHERE last_name = "鈴木"
--名前が田中でないレコード
WHERE last_name != "田中"
--名前が山田、鈴木、田中のレコード
WEHER last_name IN("山田", "鈴木", "田中")
--名前が山田、鈴木、田中でないレコード
WEHER last_name NOT IN("山田", "鈴木", "田中")
```
##### | LIKE句
e.g.
```SQL
--名前が「美○」の後方一致レコード eg.美咲
WHERE first_name LIKE "美%"
--名前が「○美」の後方一致レコード eg.愛佑美
WHERE first_name LIKE "%美"
--名前が「○美○」の部分一致レコード eg.美咲,愛佑美
WHERE first_name LIKE "%美%"
-- 名前が「(2文字)子」のレコード eg.由香子
WHERE first_name LIKE "__子"
```
ex.【4.8 演習問題1(3:22)】

```SQL
SELECT *
FROM bq_sample.customers
WHERE last_name = "前田";
```
ex.【4.8 演習問題2(4:15)】

```SQL
SELECT *
FROM `prj-test3.bq_sample.customers`
WHERE first_name IN ("愛","愛子","愛美");
```
ex.【4.8 演習問題3(5:15)】

```SQL
SELECT *
FROM `prj-test3.bq_sample.customers`
WHERE first_name NOT LIKE "%子"
ORDER BY gender desc;
```

#### ■ 日付型
e.g.
```SQL
WHERE date >= "2017-01-01"
WHERE date != "2017-01-01"
WHERE date IN("2017-07-01", "2017-07-02")
WHERE timestamp BETWEEN "2017-07-01 00:09:00" AND "2017-07-01 18:00:00"
```

ex.【4.9 演習問題1(2:00)】

```SQL
SELECT *
FROM bq_sample.shop_purchases
WHERE date <= "2018-06-01";
```
ex.【4.9 演習問題1(5:00)】

```SQL
SELECT *
FROM `prj-test3.bq_sample.shop_purchases`
WHERE date NOT BETWEEN "2018-04-29" AND "2018-05-06";
```

#### ■ 論理型
e.g.
```SQL
WHERE is_premium IS TRUE
WHERE is_premium IS FALSE
WHERE is_premium IS NOT TRUE  -- False
WHERE is_premium IS NOT FALSE -- True
```
ex.
```SQL
# 【4.10 演習問題1(1:20)】
SELECT user_id, last_name
FROM bq_sample.customers
WHERE is_premium IS FALSE;
```

### | WHERE句 - 複数条件（論理演算子）
e.g.
```sql
WHERE gender = 2 AND is_premium IS TRUE
WHERE gender = 2 AND is_premium IS TRUE AND prefecture = "北海道"
WHERE (gender = 2 AND is_premium IS TRUE) OR prefecture = "北海道"

```
ex.【4.11 演習問題1(3:20)】

```SQL
SELECT
    user_id,
    last_name,
    first_name
FROM `prj-test3.bq_sample.customers`
WHERE
    (is_premium IS FALSE AND gender = 1)
    OR
    birthday < "1970-12-31"
ORDER BY user_id
LIMIT 5;
```

### | WHERE句 - NULLの振る舞い
e.g.
```SQL
WHERE gender = 2 AND birthday IS NOT NULL
```
ex.【4.12 演習問題1(2:00)】

```SQL
SELECT user_id, last_name, first_name
FROM `prj-test3.bq_sample.customers`
WHERE first_name IS NULL;
```

### | AS句 - かラム名上書き
e.g.
```SQL
SELECT user_id,
       --dateを何の日かを説明補完できる（日 as 購入日）
       date AS purchase_data
FROM bq_sample.customers;
```
```
（メモ）
AS句は、「AS」と記載しなくとも半角スペースでも同様の挙動となる。
可読性の観点から、明記した方が親切。
```

ex.【4.13 演習問題1(1:50)】

```SQL
SELECT
    user_id,
    sales_amount AS salse_in_JPY
FROM `prj-test3.bq_sample.shop_purchases`;
```
ex.【4.13 演習問題2(3:00)】

```SQL
SELECT
    user_id,
    sales_amount AS salse_in_JPY
FROM `prj-test3.bq_sample.shop_purchases`
ORDER BY salse_in_JPY DESC
LIMIT 5;
```
ex.【4.13 演習問題2(5:00)】

(エラーを確認しよう。)
```SQL
SELECT
    user_id,
    sales_amount AS salse_in_JPY
FROM `prj-test3.bq_sample.shop_purchases`
WHERE salse_in_JPY > 10000 --error. Unrecognized name: salse_in_JPY
ORDER BY salse_in_JPY DESC
LIMIT 5;
```
```
（メモ）
WHERE句の中ではAS句で指定したカラム名は使用できない。
∵ 実行順序の関係によるため。(Cf. 5.10)
```

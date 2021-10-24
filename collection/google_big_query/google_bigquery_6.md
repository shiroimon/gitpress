---
date   : 2021-09-08
title  : 【BigQuery】分析入門 - Section6
excerpt: Google BigQuery基本の「き」について。
tags   : ["Google BigQuery", "SQL基本", "分析基本"]
---

## || Section6

### | 四則演算
e.g. `BigQueryは計算機的機能を持っている`
```SQL
SELECT 3*10;
```
| |f0_|
|-|-: |
|1| 30|


e.g. 購入ID別の売上高と、税込価格を知りたい。
```SQL
SELECT
    purchase_id,
    sales_amount,
    sales_amount * 1.08 AS sales_with_tax
FROM `prj-test3.bq_sample.shop_purchases`;
```
| |purchase_id|sales_amount|sales_with_tax   |
|-|-:         |-:          |-:               |
|1|         22|       59400|64152.00000000001|
|2|        388|       59400|64152.00000000001|

ex.【6.2 演習問題1(2:40)】

shop_purchasesテーブルから、quantityとsales_amountを利用して、purchase_idごとの平均価格を取得。
出力には、quantityとsales_amountも含める。
平均単価はavg_priceとし、昇順。
```SQL
# (miss_code)
-- SELECT purchase_id,
--        quantity,
--        sales_amount,
--        AVR(sales_amount) AS avg_price
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY purchase_id
-- ORDER BY 4 DESC;

# (collect_code)
SELECT
    purchase_id,
    quantity,
    sales_amount,
    sales_amount/quantity AS avg_price
FROM `prj-test3.bq_sample.shop_purchases`
ORDER BY 4;

-- | |purchase_id|quantity|sales_amount|avg_salse|
-- |1|        301|       2|        2660|   1330.0|
-- |2|       1040|       1|        1400|   1400.0|
-- |3|        214|       5|        7600|   1520.0|
```
ex.【6.2 演習問題2(4:10)】

直前の演習に対して、avg_priceが5000円より多い結果だけを取得。
```SQL
#(miss_code)
-- SELECT purchase_id,
--        quantity,
--        sales_amount,
--        sales_amount/quantity AS avg_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- HAVING avg_sales > 5000
-- ORDER BY 4;

#(collect_code)
SELECT
    purchase_id,
    quantity,
    sales_amount,
    sales_amount/quantity AS avg_sales
FROM `prj-test3.bq_sample.shop_purchases`
WHERE sales_amount/quantity > 5000
ORDER BY 4;

-- | |purchase_id|quantity|sales_amount|avg_salse|
-- |1|       1196|       2|       10030|   5015.0|
-- |2|       1079|       1|        5015|   5015.0|
-- |3|        619|       3|       15045|   5015.0|
```

---
### ■数列に対する基本的な関数
#### | ROOUND() - 数字を丸める関数
```SQL
SELECT
        ROUND(154.249) AS basic,-- 小数点以下第位
        ROUND(154.249, 1) AS Plus1, -- 小数点以下第二位
        ROUND(154.249, 2) AS Plus2, -- 小数点以下第三位
        ROUND(154.249, -1) AS Minus1, -- 十の位を丸める
        ROUND(154.249, -2) AS Minus2; -- 百の位を丸める
```
| |basic|Plus1|Plus2 |Minus1|Minus2|
|-|-:   |-:   |-:    |-:    |-:    |
|1|154.0|154.2|154.25| 150.0| 200.0|

#### |  CEIL(), FLOOR() - 数字を丸める関数
```
切り上げ関数： CEIL()
切り捨て関数： FLOOR()

※ いずれも戻り値が整数。
※ データ型は浮動小数点（float）なので「.0」がついている。
```

ex.【6.3 演習問題1(4:00)】


```SQL
SELECT
    purchase_id,
    sales_amount,
    sales_amount*1.08 AS Sales_w_tax,
    ROUND(sales_amount*1.08, 1) AS Round,
    CEIL(sales_amount*1.08) AS Ceil,
    FLOOR(sales_amount*1.08) AS Floor
FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY purchase_id, sales_amount    #[×]不要：but あっても同結果
ORDER BY purchase_id ASC
LIMIT 50;

-- | |purchase_id|sales_id|Salse_w_tax|Round  |Ceil   |Floor  |
-- |1|          1|  112775|    13797.0|13797.0|13797.0|13797.0|
-- |2|          2|   11600|    12528.0|12528.0|12528.0|12528.0|
```
ex.【6.3 演習問題2(6:30)】


```SQL
SELECT
    purchase_id,
    sales_amount,
    ROUND(sales_amount*1.08, -2) AS Sales_w_tax --十の位で四捨五入
FROM `prj-test3.bq_sample.shop_purchases`
ORDER BY purchase_id
LIMIT 50;

--| |purhase_id|salse_amount|Salse_w_tax|
--|1|         1|       12775|    13800.0|
--|2|         2|       11600|    12500.0|
```
ex.【6.3 x演習問題3(8:00)】


```SQL
SELECT
    purchase_id,
    sales_amount,
    -- CEIL(ROUND(sales_amount*1.08, -2)) AS Salse_w_tax  --[×] miss
    CEIL(sales_amount*1.08/100)*100 AS Salse_w_tax --十の位で切り上げ
FROM `prj-test3.bq_sample.shop_purchases`
ORDER BY purchase_id
LIMIT 50;

--| |purhase_id|salse_amount|Salse_w_tax|
--|1|         1|       12775|    13800.0|
--|2|         2|       11600|    12600.0|
```

#### | ABS() - 絶対値
ex.【6.4 演習問題1(1:30)】


```SQL
SELECT ROUND(AVG(sales_amount), 1) AS avg_salse
FROM `prj-test3.bq_sample.shop_purchases`;

--| |avg_salse|
--|1|  15693.0|

SELECT
    shop_id,
    AVG(sales_amount) AS avg_salse,
    AVG(sales_amount)-15693 AS diff,
    ABS(AVG(sales_amount)-15693) AS abs_diff
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY shop_id
ORDER BY shop_id;

--| |shop_id|avg_salse         |diff               |abs_diff          |
--|1|      1| 15810.55632582321|  117.5563258232105| 117.5563258232105|
--|2|      2|15691.087794432557|-1.9122055674433796|1.9122055674433796|
```

#### | MOD() - 割り算の余り
```
 MOD(割られる数, 割る数)
```
ex.【6.4 演習問題2(4:30)】


```SQL
SELECT *
FROM `prj-test3.bq_sample.shop_purchases`
WHERE MOD(purchase_id, 3) = 0
ORDER BY purchase_id;

--| |purchase_id|user_id|date      |shop_id|product_id|quantity|salse_amount|
--|1|          3| 876530|2018-01-01|      2|        19|       3|        4845|
--|2|          6| 954830|2018-01-01|      1|        17|       3|       15488|
```

    （用途メモ）
      数億レコードがあるデータから、ザクっと間引きしてみたい時に用いる。（ランダム抽出）
      割り切れる数字であれば、その分データ量を1/3にしたり、1/5にしたりとすることができる。


#### | CAST() - 型変換
    整数（INT64）  → 文字列（STRNG）
    少数（FLOAT64）→ 整数（INT64）

e.g.
```
CAST({対象かラム} AS {変換先データ型})
```
ex.【6.4 演習問題3(8:00)】

```SQL
SELECT
    purchase_id,
    sales_amount,
    CAST(ROUND(sales_amount*1.08, -2) AS INT64) AS sales_w_tax
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY purchase_id, sales_amount
ORDER BY 2 DESC
LIMIT 5;

--| |purchase_id|sales_amount|sales_tax|
--|1|       1203|       99000|   106900|
--|2|        995|       99000|   106900|
```
---
### ■文字列に対する基本的な関数
#### | CONCAT() - 文字列連結
e.g.
```
CONCAT({文字列}, {文字列}, {文字列}・・・)
```
ex.【6.5 演習問題1(1:50)】


```SQL
SELECT
    user_id,
    CONCAT(last_name, " ", first_name) AS full_name
FROM `prj-test3.bq_sample.customers`
ORDER BY 1;

-- |  |user_id|full_name|
-- | 1| 496070|片桐 雅友|
-- | 2| 496070|才村 歩加|
-- |21| 613645|#N-A 守  |
```
ex.【6.5 演習問題2(3:10)】


```SQL
SELECT
    purchase_id,
    CONCAT(CAST(user_id AS STRING), "-", CAST(shop_id AS STRING)) AS user_shop,
    sales_amount
FROM `prj-test3.bq_sample.shop_purchases`
ORDER BY purchase_id
LIMIT 10;

-- | |purchase_id|user_shop|sales_amount|
-- |1|          1|733995-2 |       12775|
```
#### | LENGTH() - 文字列の文字数取得
ex.【6.5 演習問題3(5:30)】


```SQL
SELECT
    product_id,
    prod_name,
    LENGTH(prod_name) AS letters
FROM `prj-test3.bq_sample.products_master`
ORDER BY 3;

--| |product_id|prod_name    |letters|
--|1|         9|ブラウス 半袖|      7| ※半角スペースもカウント
```

#### | SUBSTR() - 文字列の一部取得
e.g. prod_nameの１番左から、5文字取得
```
SUBSTR(prod_name, 1, 5)
```

ex.【6.5 演習問題4(8:00)】


```SQL
SELECT
    product_id,
    prod_name,
    SUBSTR(prod_name, -6, 6) AS right6 --右から6文字
FROM `prj-test3.bq_sample.products_master`
ORDER BY product_id;

--| |product_id|prod_name                      |right6 |
--|1|         1|オックスフォードシャツ 綿 100%|綿 100%|
```

#### | REPLACE() - 文字列の一部置き換え
e.g. ing(現在進行形) を ed(過去形) に置き換える
```
REPLACE("creating", "ing", "ed")
```
ex.【6.5 演習問題5(10:10)】


```SQL
SELECT
    product_id,
    prod_name,
    REPLACE(prod_name, "半袖", "七分袖") AS prod_name
FROM `prj-test3.bq_sample.products_master`
ORDER BY product_id;

--| |product_id|prod_name       |prod_name_1     |
--|7|         7|ポロシャツ 半袖|ポロシャツ 七分袖|
--|8|         8|ポロシャツ 長袖|ポロシャツ 長袖  |
```

#### | TRIM() - 文字列の前後の空白、任意の文字数削除
e.g.
```
TRIM({文字列}, {削除したい文字})

-- 文字列前後の＄マークを削除したい
TRIM(文字列,"$")
```
ex.【6.5 演習問題6(12:00)】


```SQL
SELECT
    " 平成 ",
    TRIM(" 平成 ");

--| |f0_   |f1_ |
--|1| 平成 |平成|

SELECT
    "https://www.udemy.com",
    TRIM("https://www.udemy.com", "https://");

--| |f0_                  |f1_          |
--|1|https://www.udemy.com|www.udemy.com|
```

### ■正規表現
（BigQueryには正規表現を利用できる関数がある。）
#### | REGEXP_CONTAINS() - 指定正規表現の含有の有無
e.g.
```SQL
REGEXP_CONTAIN(文字列, r"正規表現")

/*文字列に [?] が入っているか否か確認。*/
SELECT
    REGEXP_CONTAINS("/products/?id=15", r"\?"),
    REGEXP_CONTAINS("/company/", r"\?");
    --Windowsでは \ ではなく ¥
```
| |f0_ |f1_  |
|-|-:  |-:   |
|1|true|false|

ex.【6.6 演習問題1(2:50)】


```SQL
#(miss_code)
-- SELECT
--     REGEXP_CONTAINS(page, r"\?") IS TRUE AS prob_dir,
--     SUM(pageview) AS PV
-- FROM `prj-test3.bq_sample.web_usage`
-- GROUP BY 1;
-- | |prob_dir|PV  |
-- |1|false   | 653|
-- |2|true    |1945|

#(collect_code)
SELECT SUM(pageview) AS PV
FROM `prj-test3.bq_sample.web_usage`
WHERE REGEXP_CONTAINS(page, r"^/products/\?id=[0-9][0-9]?$") IS TRUE;
--[0-9]:0~9数字が1つ、 ?：直前の文字があってもなくても良いの意味

-- | |PV  |
-- |1|1944|
```

#### | REGEXP_EXTRACT() - 文字列抽出
e.g.
```
REGEXP_EXTRACT(value, regex [, position[, occurrence]])
```
```sql
SELECT
    -- ^:始めから、[^@]:＠でないところまで、+：1つ以上の繰り返す
    REGEXP_EXTRACT("sample@gmail.com", r"^[^@]+"),
    -- -()-:ハイフンに囲まれた、\d:数字、+：1つ以上の繰り返す
    REGEXP_EXTRACT("03-1234-5678", r"-(\d+)-"),
    -- /の後に、\s:半角スペース、(.+):どんな文字列でも1つ以上の繰り返されたもの、$:最後まで
    REGEXP_EXTRACT("google / organic", r"/\s(.+)$");
```
| |f0_   |f1_ |f2_    |
|-|:-    |-:  |:-     |
|1|sample|1234|organic|


#### | REGEXP_REPLACE() - 置き換え
e.g.
```
REGEXP_REPLACE(value, regex, replacement)
```
```SQL
SELECT
    -- [a]と、[.]:何でもいいと、[c]の文字列
    REGEXP_REPLACE("abc@gmail.com", r"a.c", "XYZ"),
    REGEXP_REPLACE("bbc@gmail.com", r"a.c", "XYZ"),
    -- [^@]:＠マークでない全ての文字
    REGEXP_REPLACE("abc@gmail.com", r"[^@]", "X"),
    REGEXP_REPLACE("www.sample.com/products/", r"(^.+\.com)", "sample.jp"),
    REGEXP_REPLACE("www.sample.ac.jp/products/", r"(^.+\.com)", "sample.jp");
```
| |f0_|f1_|f2_|f3_|f4_|
|-|:- |:- |:- |:- |:- |
|1|XYZ@gmail.com|bbc@gmail.com|XXX@XXXXXXXXX|sample.jp/products/|www.sample.ac.jp/products/|

---
### ■日時、日付に対する基本的な関数
#### | DATE()、DATETIME() - DATE型(DATETIME型)
e.g.
```SQL
DATE(2021, 9, 30) --2021/09/30
DATETIME(2021,9,30,12,35,15) --2021/09/30 12:35:15
```
ex.【6.7 演習問題1(1:50)】


```SQL
SELECT
    DATE(2019, 4, 30) AS date,
    DATETIME(2019, 4, 30, 13, 22, 15) AS datetime;

-- | |date      |datetime           |
-- |1|2019-04-30|2019-04-30T13:22:15|
```
```
(型確認:スキーマ)
 実行後 -> ジョブ情報（結果画面横）-> 一時テーブル（ページ最下部）-> テーブルスキーマーを確認。

    |フィールド名|種類     |モード  |
    |date        |DATE     |NULLABLE|
    |datetime    |DATETIME |NULLABLE|
```
ex.【6.7 演習問題2(4:30)】


```SQL
SELECT
    purchase_id,
    DATETIME(date) AS datetime
FROM `prj-test3.bq_sample.shop_purchases`
ORDER BY purchase_id
LIMIT 10;

-- | |purchase_id|datetime           |
-- |1|          1|2018-01-01T00:00:00|
```
ex.【6.7 演習問題3(5:30)】


```SQL
SELECT
    timestamp,
    DATE(timestamp) AS date
FROM `prj-test3.bq_sample.web_usage`
ORDER BY 1
LIMIT 100;

-- | |timestamp          |date      |
-- |1|2018-01-01T13:12:57|2018-01-01|
```

#### | CURRENT_DATE()、CURRENT_DATETIME() - 現在データの取得
e.g.  ※ 現在時刻をUTC（協定世界時：日本より9時間前）で取得する。
```SQL
SELECT
    CURRENT_DATE("+09:00"),
    CURRENT_DATETIME("+09:00");
```
ex.【6.7 演習問題5(9:15)】


```SQL
SELECT
    CURRENT_DATE("Asia/Tokyo"),
    CURRENT_DATETIME("Asia/Tokyo");
```

#### | DATE_ADD()、DATETIME_ADD() - 追加
e.g.
```sql
DATE_ADD(date_expr, INTERVAL int64_expr date_part)

/*2019年5月3日に３週間を加算する。*/
SELECT DATE_ADD(date"2019-05-03", INTERVAL 3 week);
```
```
(date_part)
※ DATE    : date_part = [yera, quarter, month, week, day]
※ DATETIME: date_part = [yera, quarter, month, week, day]
                        +[hour, minute , second, milisecond, microsecond]
```
ex.【6.8 演習問題1(2:15)】


```SQL
SELECT
    DATE_ADD(CURRENT_DATE("Asia/Tokyo"), INTERVAL 3 month);

-- | |f0_       |
-- |1|2021-12-6|
```
    (memo) 例えば、3ヶ月前とかも取れる。その場合は「-3」

ex.【6.8 演習問題2(3:50)】


```SQL
SELECT DATETIME_ADD(CURRENT_DATETIME("+9:00"), INTERVAL 6 hour);

-- | |f0_                       |
-- |1|2021-09-07T04:21:28.704252|
```

### | DATE_SUB(), DATETIME_SUB() - 差し引く
e.g.
```SQL
SELECT DATE_SUB(date_expr, INTERVAL int64_expr date_part);
```

### | DATE_DIFF()、DATETIME_DIFF() - 差分
e.g.
```SQL
SELECT DATE_DIFF(date_expr, date_expr, date_part);
```
ex.【6.8 演習問題3(6:30)】


```SQL
SELECT DATE_DIFF(date"2019-04-30", date"1989-01-08", day);

-- | |f0_  |
-- |1|11069|
```
ex.【6.8 演習問題4(7:30)】


```SQL
SELECT DATETIME_DIFF(datetime"2019-05-02T10:12:15", datetime"2019-05-02T09:45:23", second);

-- | |f0_ |
-- |1|1612|
```

#### | DATE_TRUNC(), DATETIME_TRUNC() - 切り詰める
（=Truncate）

e.g.
```SQL
SELECT DATE_TRUNC(date_expr, date_part)
```
```SQL
SELECT DATE_TRUNC("1989-01-15", month);
```
| |f0_       |
|-|-|
|1|1989-01-01|

`↑月以降は切り詰められている`

```SQL
 SELECT DATETIME_TRUNC("1989-05-02 06:39:25", day);
```
| |f0_               |
|-|-                 |
|1|1989-05-02 0:00:00|

`日付以降は切り詰められている`

ex.【6.9 演習問題1(2:10)】


```SQL
SELECT
    DATE_TRUNC(date, MONTH) AS month,
    SUM(sales_amount) AS total_sales
FROM  `prj-test3.bq_sample.shop_purchases`
GROUP BY 1
ORDER BY 1;

-- | |month     |total_sales|
-- |1|2018-01-01|    2056764|
-- |2|2018-02-01|    1133128|
```

#### | FORMAT_DATE(), FORMAT_DATETIME() - 出力フォーマット
e.g.
```SQL
SELECT FORMAT_DATE(format_string, date);
```
```SQL
SELECT
    FORMAT_DATE("%F", "2019-05-02") AS F_upper,
    FORMAT_DATE("%x", "2019-05-02") AS x_lower,
    FORMAT_DATE("%Y", "2019-05-02") AS Y_upper,
    FORMAT_DATE("%y", "2019-05-02") AS y_lower,
    FORMAT_DATE("%m", "2019-05-02") AS m_lower,
    FORMAT_DATE("%d", "2019-05-02") AS d_lower,
    FORMAT_DATE("%A", "2019-05-02") AS A_upper,
    FORMAT_DATE("%a", "2019-05-02") AS a_lower,
    FORMAT_DATE("%B", "2019-05-02") AS B_upper,
    FORMAT_DATE("%b", "2019-05-02") AS b_lower;
```
| |F_upper   |x_lower |Y_upper|y_lower|m_lower|d_lower|A_upper |a_lower|B_upper|b_lower|
|-|-|-|-|-|-|-|-|-|-|-|
|1|2019-05-02|05/02/19|   2019|     19|     05|     02|Thursday|Thu    |May    |May    |

```SQL
SELECT
    FORMAT_DATETIME("%c", "2019-05-02 09:29:41") AS c_lower,
    FORMAT_DATETIME("%r", "2019-05-02 09:29:41") AS r_lower,
    FORMAT_DATETIME("%X", "2019-05-02 09:29:41") AS X_upper,
    FORMAT_DATETIME("%H", "2019-05-02 09:29:41") AS H_upper,
    FORMAT_DATETIME("%M", "2019-05-02 09:29:41") AS M_upper,
    FORMAT_DATETIME("%S", "2019-05-02 09:29:41") AS S_upper,
    FORMAT_DATETIME("%P", "2019-05-02 09:29:41") AS P_upper,
    FORMAT_DATETIME("%s", "2019-05-02 09:29:41") AS s_lower,
    FORMAT_DATETIME("%p", "2019-05-02 09:29:41") AS p_lower;
```
| |c_lower|r_lower|X_upper|H_upper|M_upper|S_upper|P_upper|s_lower|p_lower|
|-|-|-|-|-|-|-|-|-|-|
|1|Thu May 2 09:29:41 2019|09:29:41 AM|09:29:41|09     |29     |41     |am     |1556789381|AM     |

#### | MAX(), MIN() - 最大値、最小値
ex.【6.9 演習問題2(4:00)】


```SQL
SELECT
    shop_id,
    -- MIN(DATE_TRUNC(date, DAY)) AS oldest,
    -- MAX(DATE_TRUNC(date, DAY)) AS newest,
    -- MAX(DATE_TRUNC(date, DAY))-MIN(DATE_TRUNC(date, DAY)) AS days_diff
    MIN(date) AS oldest,
    MAX(date) AS newest,
    DATE_DIFF(MAX(date), MIN(date), DAY) AS days_diff
FROM `prj-test3.bq_sample.shop_purchases`
WHERE shop_id = 4
GROUP BY shop_id;

-- | |shop_id|oldest    |newest    |days_diff|
-- |1|      4|2018-01-05|2018-12-19|      348|
```
ex.【6.9 演習問題3(9:00)】


```SQL
#(miss code)
-- SELECT
--     user_id,
--     MIN(DATETIME_TRUNC(timestamp, DAY)) AS oldest,
--     MAX(DATETIME_TRUNC(timestamp, DAY)) AS newest,
--     DATETIME_DIFF(
--         MAX(DATETIME_TRUNC(timestamp, DAY)),
--         MIN(DATETIME_TRUNC(timestamp, DAY)),
--         DAY) AS days_diff,
--     session_count
--     -- AVG() AS avg_visit
-- FROM `prj-test3.bq_sample.web_usage`
-- GROUP BY user_id, session_count
-- HAVING DATETIME_DIFF(
--         MAX(DATETIME_TRUNC(timestamp, DAY)),
--         MIN(DATETIME_TRUNC(timestamp, DAY)),
--         DAY) > 1
-- ORDER BY days_diff;
#(collect_code)
SELECT
    user_id,
    MIN(timestamp) AS oldest,
    MAX(timestamp) AS newest,
    DATETIME_DIFF(MAX(timestamp), MIN(timestamp), DAY) AS days_diff,
    COUNT(DISTINCT session_count) AS visit_count, --[重複session数を除いて、ユニークなsession数を取得]
    DATETIME_DIFF(MAX(timestamp), MIN(timestamp), DAY)/COUNT(DISTINCT session_count) AS avg_interval
FROM `prj-test3.bq_sample.web_usage`
GROUP BY user_id
HAVING
    DATETIME_DIFF(MAX(timestamp), MIN(timestamp), DAY) <> 0
    AND COUNT(DISTINCT session_count) <> 1
ORDER BY avg_interval;

-- | |user_id|oldest             |newest             |days_diff|visit_count|avg_interval|
-- |1| 916550|2018-10-22T00:46:50|2018-10-23T14:34:55|        1|          8|       0.125|
-- |2|1011670|2018-10-04T18:37:15|2018-10-05T13:42:21|        1|          4|        0.25|
```

---
### 様々な条件式
#### | IF文 - 条件式
e.g.
```SQL
IF(条件式, True, False)
```
```SQL
SELECT
    product_id,
    prod_name,
    IF(product_id >= 11, "Tシャツ", "それ以外") AS category
FROM `prj-test3.bq_sample.products_master`;
```
|  |product_id|prod_name                     |category|
|- |-:        |:-                            |:-      |
|1 |         1|オックスフォードシャツ 綿 100%|それ以外|
|11|        11|Tシャツ（デザイン） 半袖      |Tシャツ |

ex.【6.10 演習問題1(3:20)】


```SQL
SELECT
    IF(product_id >= 11, "Tシャツ", "Tシャツ以外") AS category,
    SUM(quantity) AS total_pty
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY 1;

-- | |category    |total_qty|
-- |1|Tシャツ以外 |     1156|
-- |2|Tシャツ     |     1739|
```

#### | IFNULL文 - nullに対しての条件式
e.g. nullの時に発動(※ 値がnullでないなら何もしない)
```sql
IFNULL(かラム名, nullの場合の返り値)
```
cf. [値がNULLだった場合は指定した別の値を返す(ifnull関数, coalesce関数)](https://www.dbonline.jp/sqlite/function/index23.html) - DBOnline

ex.【6.10 演習問題2(5:20)】


```SQL
SELECT
    *,
    IFNULL(gender, 3) AS gender_not_null
FROM `prj-test3.bq_sample.customers`
ORDER BY 1;

-- | |user_id||gender|~|gender_not_null|
-- |1| 496070||     1|~|              1|
-- |7| 596992||null  |~|              3|
```

#### | CASE文 - 条件式
e.g.
```SQL
CASE (判定条件)  --Bool判定式では記載無いこともある
   WHEN 値 THEN 値が合致した場合の戻り値
   WHEN 値 THEN 値が合致した場合の戻り値
   WHEN 値 THEN 値が合致した場合の戻り値
   ELSE いずれにも合致しなかった場合の戻り値
END
```
```SQL
SELECT
    user_id,
    gender,
    CASE gender
        WHEN 1 THEN "男性"
        WHEN 2 THEN "女性"
        ELSE "不明"
    END AS gender_name
FROM `prj-test3.bq_sample.customers`
ORDER BY 1;
```
| |user_id|gender|gender_name|
|-|-:|-:|:-|
|1| 496070|     1|男性        |
|2| 497520|     2|女性        |
|7| 596992|null  |不明        |

```SQL
SELECT
    user_id,
    prefecture,
    CASE
        WHEN REGEXP_CONTAINS(prefecture, r"Tokyo|Kanawa") THEN "東京他" -- |:または
        WHEN REGEXP_CONTAINS(prefecture, r"Osaka|Hyogo") THEN "大阪他"
        ELSE "その他"
    END AS pref_category
FROM `prj-test3.bq_sample.customers`
ORDER BY 1;
```
| |user_id|prefecture|pref_category |
|-|-:     |:-        |:-            |
|1| 496070|Tokyo     |東京他        |
|4| 529710|Fukuoka   |その他        |
|9| 597427|Hyogo     |大阪他        |

ex.【6.11 演習問題1(6:30)】


```SQL
-- SELECT
--     sales_amount,
--     ROUND(sales_amount, -3)
-- FROM `prj-test3.bq_sample.shop_purchases`;
#(miss_code)
-- SELECT
--     purchase_id,
--     sales_amount,
--     CASE
--         WHEN ROUND(sales_amount, -3) = 0 THEN "１万円未満"
--         WHEN ROUND(sales_amount, -3) = 10000 THEN "１万円"
--         WHEN ROUND(sales_amount, -3) = 20000 THEN "２万円"
--         ELSE "３万円以上"
--     END AS round_sales
-- FROM `prj-test3.bq_sample.shop_purchases`
-- ORDER BY 1;
#(collect_code)
SELECT
    purchase_id,
    sales_amount,
    CASE ROUND(sales_amount, -4)
        WHEN 0 THEN "１万円未満"
        WHEN 10000 THEN "１万円"
        WHEN 20000 THEN "２万円"
        ELSE "３万円以上"
    END AS round_sales
FROM `prj-test3.bq_sample.shop_purchases`;

-- |  |purchase_id|sales_amount|round_sales|
-- | 1|         22|       59400|３万円以上 |
-- | 5|         17|       19000|２万円     |
-- |74|        335|       12580|１万円     |
```
ex.【6.11 演習問題2(11:30)】


```SQL
#(miss_code)
-- SELECT
--     CASE product_id
--         WHEN 1 BETWEEN 6 THEN "男性衣類"
--         WHEN 7 BETWEEN 10 THEN "女性衣類"
--         WHEN 11 BETWEEN 18 THEN "Tシャツ"
--         WHEN IN(19, 20) THEN "子供用Tシャツ"
--         ELSE "不明"
--     END AS prod_category,
--     AVG(sales_amount) AS avg_amount
-- FROM `prj-test3.bq_sample.shop_purchases`
-- GROUP BY prod_category
-- ORDER BY 2;

#(colect code)
SELECT
    CASE
        WHEN product_id <= 6 THEN "男性衣類"
        WHEN product_id <= 10 THEN "女性衣類"
        WHEN product_id <= 18 THEN "Tシャツ"
        ELSE "子供用Tシャツ"
    END AS prod_category,
    ROUND(AVG(sales_amount), 2)AS avg_amount
FROM `prj-test3.bq_sample.shop_purchases`
GROUP BY 1
ORDER BY 2 DESC;

-- | |prod_category|avg_amount|
-- |1|男性衣類      |  30074.72|
-- |2|女性衣類      |  24052.13|
```

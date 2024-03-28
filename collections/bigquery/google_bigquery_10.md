---
date    : 2021-09-04
title   : 🔍 分析入門 Section10: 集合演算、ビュー
excerpt : ---
tags    : ["Google BigQuery", "SQL", "分析", "union", "Udemy講座"]
---


![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)


## || Section10
### | テーブル同士の集合演算

```
1. 和集合 A∩B（AかつB）
    →重複を除外しないパターン、重複を除外するパターン
```
```
2. 積集合 A∪B（AまたはB）
    →重複部分のみ
```
```
3. 差集合 A∩!B（AかつノットB）
    →重複と該当データ以外を差し引く
```

### | UNION - 和集合
> ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Venn_A_union_B.png/220px-Venn_A_union_B.png)
> * Cf. [和集合 - Wikipedia](https://ja.wikipedia.org/wiki/%E5%92%8C%E9%9B%86%E5%90%88)

e.g.
```SQL
SELECT [column] FROM [table1]
UNION (ALL | DISTINCT)
SELECT [column] FROM [table2]

※ 同じ列数
※ 同じ列の順序で（同じ列名であっても順番まで問われる）
※ 重複を無視：UNION ALL（[table1]+[table2]の列数）
※ 重複を除外：UNION DISTINCT（[table1]+[table2]-[dist]の列数）
```
ex.【10.3 演習問題1(5:30)】


```SQL
-- SELECT COUNT(*) FROM `prj-test3.bq_trial.event_jan`; --5record
-- SELECT COUNT(*) FROM `prj-test3.bq_trial.event_feb`; --6record
SELECT * FROM `prj-test3.bq_trial.event_jan`
UNION ALL
SELECT * FROM `prj-test3.bq_trial.event_feb`;

-- | |date      |place|last_name|first_name|gender|age|
-- |1|2019-02-15|渋谷 |山本     |大輔      |男性  | 29|
-- |2|2019-02-15|渋谷 |山田     |太郎      |男性  | 28|
-- |9|2019-01-15|池袋 |山田     |太郎      |男性  | 28|
```

eg. 2つ以上のテーブルUNION
```SQL
SELECT [column] FROM [table1]
UNION (ALL | DISTINCT)
SELECT [column] FROM [table2]
UNION (ALL | DISTINCT)
SELECT [column] FROM [table3]
```
eg. UNIONタイプを複合
→ ([table1]+[table2])+[table3]-[dist]
```SQL
(SELECT [column] FROM [table1]
 UNION ALL
 SELECT [column] FROM [table2])
UNION DISTINCT
SELECT [column] FROM [table3]
```
ex.【10.3 演習問題2(8:50)


```SQL
SELECT * FROM `prj-test3.bq_trial.event_jan`
UNION ALL
SELECT * FROM `prj-test3.bq_trial.event_feb`
UNION ALL
SELECT * FROM `prj-test3.bq_trial.event_mar` ORDER BY age;

--| |date      |place|last_name|first_name|gender|age|
--|1|2019-02-15|渋谷 |高田     |みすず    |女性  | 20|
--|2|2019-01-15|池袋 |山田     |華子      |女性  | 25|
--|3|2019-02-15|渋谷 |山田     |太郎      |男性  | 28|

# サブクエリを用いて、親クエリでフィルタリングする。
SELECT *
FROM
    (SELECT * FROM `prj-test3.bq_trial.event_jan`
     UNION ALL
     SELECT * FROM `prj-test3.bq_trial.event_feb`
     UNION ALL
     SELECT * FROM `prj-test3.bq_trial.event_mar`)
WHERE gender="女性"
ORDER BY age;

--| |date      |place|last_name|first_name|gender|age|
--|1|2019-02-15|渋谷  |高田    |みすず    |女性  | 20|
--|2|2019-01-15|池袋  |山田    |華子      |女性  | 25|
--|3|2019-03-15|品川  |高橋    |純子      |女性  | 28|
```
ex.【10.3 演習問題3(13:20)】

```sql
#(miss_code)
-- SELECT * FROM `prj-test3.bq_trial.event_jan`
-- UNION DISTINCT
-- SELECT * FROM `prj-test3.bq_trial.event_feb`;
--| |date      |place|last_name|first_name|gender|age|
--|1|2019-02-15|渋谷 |山本     |大輔      |男性  | 29|
--|2|2019-02-15|渋谷 |山田     |太郎      |男性  | 28|
--|3|2019-02-15|渋谷 |本田     |健太郎    |男性  | 35|

 #(collect_code)
SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_jan`
UNION DISTINCT
SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_feb`;

--| |last_name|first_na|gender|age|
--|1|山本     |大輔    |男性  | 29|
--|2|山田     |太郎    |男性  | 28|
```
ex.【10.3 演習問題4(17:20)】


```sql
SELECT
    COUNT(*)
FROM
    (SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_jan`
     UNION DISTINCT
     SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_feb`
     UNION DISTINCT
     SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_mar`)
;

--| |f0_|
--|1| 14|
```

### | INTERSECT - 積集合
> ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2d/Interseccion.svg/250px-Interseccion.svg.png)
> * Cf. [共通部分（数学） - Wikipedia](https://ja.wikipedia.org/wiki/%E5%85%B1%E9%80%9A%E9%83%A8%E5%88%86_(%E6%95%B0%E5%AD%A6))
> * Cf. [ド・モルガンの法則 - Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%89%E3%83%BB%E3%83%A2%E3%83%AB%E3%82%AC%E3%83%B3%E3%81%AE%E6%B3%95%E5%89%87)

e.g.
```SQL
SELECT [column] FROM [table1]
INTERSECT DISTINCT
SELECT [column] FROM [table2]

※ 同じ列数
※ 同じ列の順序で（同じ列名であっても順番まで問われる）
```

ex.【10.4 演習問題1(1:50)】


```SQL
SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_jan`
INTERSECT DISTINCT
SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_feb`;

--| |last_name|first_name|gender|age|
--|1|山田     |太郎      |男性  | 28|
```
ex.【10.4 演習問題1(1:50)】


```sql
SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_jan`
INTERSECT DISTINCT
SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_feb`
INTERSECT DISTINCT
SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_mar`;

-- | |last_name|first_name|gender|age|
-- |1|山田     |太郎      |男性  | 28|
```
ex.【10.4 演習問題3(5:20)】


```sql
(SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_jan`
 UNION DISTINCT
 SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_feb`)
INTERSECT DISTINCT
SELECT last_name,first_name,gender,age FROM `prj-test3.bq_trial.event_mar`;

-- | |last_name|first_name|gender|age|
-- |1|山田     |太郎     |男性   | 28|
-- |2|高橋     |純子     |女性   | 28|
```

### | EXCEPT - 差集合
> ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Venn0100.svg/220px-Venn0100.svg.png)
> * Cf. [差集合 - Wikipedia](https://ja.wikipedia.org/wiki/%E5%B7%AE%E9%9B%86%E5%90%88)

e.g.
```SQL
SELECT [column] FROM [table1]
EXCEPT DISTINCT
SELECT [column] FROM [table2]

※ 同じ列数
※ 同じ列の順序で（同じ列名であっても順番まで問われる）
※ テーブルの順序によって引き算した場合結果は異なる。（eg. 5行-2行=2行, 2行-5行=-2行）
```
ex.【10.5 演習問題1(2:40)】


```SQL
SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_feb`
EXCEPT DISTINCT
SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_jan`;

-- | |last_name|first_name|gender|age|
-- |1|山本     |大輔      |男性  | 29|
-- |2|本田     |健太郎    |男性  | 35|
```
ex.【10.5 演習問題2(4:50)】


```sql
SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_mar`
EXCEPT DISTINCT
(SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_jan`
 UNION DISTINCT
 SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_feb`);

-- | |last_name|first_name|gender|age|
-- |1|鈴木      |輝夫     |男性  | 30|
-- |2|橋田      |睦       |男性  | 30|
-- 計4名
```
ex.【10.5 演習問題3(6:50)】


```sql
#(miss_code)
-- (SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_mar`
-- UNION DISTINCT
-- SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_jan`)
-- EXCEPT DISTINCT
-- SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_feb`;
--
--| |last_name|first_name|gender|age|
--|1|鈴木     |輝夫      |男性  | 30|
--|2|橋田     |睦        |男性  | 30|
-- 計8名

#(collect_code)
(SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_mar`
 INTERSECT DISTINCT
 SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_jan`)
EXCEPT DISTINCT
SELECT last_name, first_name, gender, age FROM `prj-test3.bq_trial.event_feb`;

--| |last_name|first_name|gender|age|
--|1|高橋     |純子      |女性  | 28|
```

---
### | ビュー
```SQL
ある程度複雑化した、SQLを呼び出せる形で保存ができる便利機能。
テーブルの様な振る舞いではあるが、実際ではSQLである。

eg. SELECT *
    FROM `prj-test3.bq_sample.shop_purchases` AS sp
    INNER JOIN `prj-test3.bq_sample.products_master` AS pm USING(product_id);
```


1. ビューの保存（作成）
![img](https://i.gyazo.com/1df5f397b46f95b369f57ad92b592957.png)
```
記載したSQLを保存のメニューバーから、「ビューを保存」を選択。
```

2. 保存名設定
![img](https://i.gyazo.com/e6c369473d2b6c0fc020e836c003e587.png)
```
保存先や、テーブル名を設定。
（ここでは、`joined_sp_pm`とする。）
```

3. ビューの確認
![img](https://i.gyazo.com/b8ac13d91a7b5ec9798445272955f62e.png)
```
左側のプロジェクト一覧から、ドリルダウンしていくと、指定したビューが作成されている。
```

4. ビューの呼び出し
```SQL
SELECT * FROM  `prj-test3.bq_sample.joined_sp_pm`;
```

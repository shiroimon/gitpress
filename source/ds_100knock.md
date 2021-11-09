---
date    : 2021-09-24
title   : 【📊 DS】データサイエンス100本ノック（構造化データ加工編）
excerpt : データサイエンス協会が、データサイエンス初学者のための実践的な学習環境をGitHubに無料公開！
tags    : ["DataScientist", "GitHub", "Docker", "SQL", "R", "Python"]
---

## || なにこれ
> 一般社団法人データサイエンティスト協会（所在地：東京都港区、代表理事：草野 隆史、以下データサイエンティスト協会）は、構造化データの加工について実践的に学ぶことができる無料の学習環境「データサイエンス100本ノック（構造化データ加工編）」をGitHubに公開しました。
>
>「データサイエンス100本ノック（構造化データ加工編）」は、データサイエンス初学者を対象に、データの加工・集計、統計学や機械学習を駆使したモデリングの前処理等を学べるよう、データと実行環境構築スクリプト、演習問題をワンセットにしています。
>
> _ー_ [データサイエンス初学者のための実践的な学習環境 「データサイエンス100本ノック（構造化データ加工編）」をGitHubに無料公開](https://digitalpr.jp/r/39499) - Digital PR Platform

GitHubは[こちら. (The-Japan-DataScientist-Society/100knocks-preprocess)](https://github.com/The-Japan-DataScientist-Society/100knocks-preprocess)

---
## || 準備（環境構築）
#### | 構築イメージ
> ![img](https://user.pr-automation.jp/simg/1731/39499/700_277_202006151645535ee727319bac0.JPG)
> ー [Digital PR Platform](https://digitalpr.jp/r/39499)

#### | 🐙GitHub
`$ mkdir plactice` 任意のディレクトリを作成。
`$ cd plactice/`移動して、
```SHELL
$ git clone https://github.com/The-Japan-DataScientist-Society/100knocks-preprocess
```

#### | 🐋Docker
`$ cd 100knocks-preprocess/`移動して、
```SHELL
$ docker-compose up -d --build
```
* _Cf :_ [Docker入門して機械学習環境構築](https://karaage.hatenadiary.jp/entry/2019/05/17/073000) - からあげ

#### | 🪐Jupyter Lab
```SHELL
$ jupyter lab
```

* _Cf :_ [【データサイエンス】データサイエンス100本ノック！実装までの道程](https://www.youtube.com/watch?v=mh8Z5d0-0PU) - Youtube
* _Cf :_ [Macでデータサイエンス100本ノックを動かす方法](https://qiita.com/karaage0703/items/1b18b1f4ab65d35afb5f)- Qiita
* _Cf :_ [データサイエンス100本ノックを、Google ColabとAzure Notebooksで気軽に行いたい！](https://qiita.com/noguhiro2002/items/de49db61b69c3dbc9282)- Qiita
→環境構築 "怠ッ.." な人向け。コモン化した最強の記事(ありがたい)。


---
## || やってみる
### | 1.
レシート明細のデータフレーム（df_receipt）から全項目の先頭10件を表示し、どのようなデータを保有しているか目視で確認せよ。
```python
df_receipt.head(10)
```
```SQL
SELECT * FROM df_receipt LIMIT 10;
```
### | 2.
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、10件表示させよ。
```python
df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]].head(10)
```
```SQL
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM df_receipt
LIMIT 10;
```
### | 3.
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、10件表示させよ。ただし、sales_ymdはsales_dateに項目名を変更しながら抽出すること。
```python
df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]].rename(columns={"sales_ymd":"sales_date"}).head(10)
```
```SQL
SELECT
    sales_ymd AS sales_date,
    customer_id,
    product_cd,
    amount
FROM df_receipt
LIMIT 10;
```
### | 4.
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
- 顧客ID（customer_id）が"CS018205000001"

```python
df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]].query('customer_id == "CS018205000001"')
```
```SQL
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM df_receipt
WHERE customer_id = "CS018205000001";
```
### | 5.
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上

```python

```
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM df_receipt
WHERE customer_id = "CS018205000001" AND amount >= 1000;
```
### | 6.
レシート明細データフレーム「df_receipt」から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上数量（quantity）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上または売上数量（quantity）が5以上

```python
```
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    quantity,
    amount
FROM df_receipt
WHERE
    customer_id = "CS018205000001"
    AND
    (amount >= 1000 OR quantity >= 5);
```
### | 7.
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上2,000以下

```python
```
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM df_receipt
WHERE
    customer_id = "CS018205000001"
    AND
    amount BETWEEN 1000 AND 2000 ;
```
### | 8.
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 商品コード（product_cd）が"P071401019"以外

```python
```
```sql
SELECT
    sales_ymd,
    customer_id,
    product_cd,
    amount
FROM df_receipt
WHERE
    customer_id = "CS018205000001"
    AND
    product_cd <> "P071401019";
```
### | 9.
以下の処理において、出力結果を変えずにORをANDに書き換えよ。

`df_store.query('not(prefecture_cd == "13" | floor_area > 900)')`

```python
```
```sql
```
### | 10.
店舗データフレーム（df_store）から、店舗コード（store_cd）が"S14"で始まるものだけ全項目抽出し、10件だけ表示せよ。
```python
```
```sql
SELECT *
FROM df_store
WHERE store_cd LIKE "S14%"
-- WHERE REGEXP(store_cd, r"S14")
LIMIT 10;
```
### | 11.
顧客データフレーム（df_customer）から顧客ID（customer_id）の末尾が1のものだけ全項目抽出し、10件だけ表示せよ。
```python
```
```sql
SELECT *
FROM df_customer
WHERE REGEXP_CONTAIN(customer_id, r"1$")
LIMIT 10;
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```
### | 0.
```python
```
```sql
```

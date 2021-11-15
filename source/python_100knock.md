---
date    : 2021-09-26
title   : 【🐍 Python】データサイエンス100本ノック（構造化データ加工編）
excerpt : データサイエンス協会が、データサイエンス初学者のための実践的な学習環境をGitHubに無料公開！
tags    : ["DataScientist", "GitHub", "Docker", "Python"]
---
## Python編
### | P-001
レシート明細のデータフレーム（df_receipt）から全項目の先頭10件を表示し、どのようなデータを保有しているか目視で確認せよ。
```python
df_receipt.head(10)
```

### | P-002
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、10件表示させよ。
```python
df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]].head(10)
```

### | P-003
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、10件表示させよ。ただし、sales_ymdはsales_dateに項目名を変更しながら抽出すること。
```python
df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]].rename(columns={"sales_ymd":"sales_date"}).head(10)
```

### | P-004
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
- 顧客ID（customer_id）が"CS018205000001"

```python
df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]].query('customer_id == "CS018205000001"')
```

### | P-005
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上

```python

```

### | P-006
レシート明細データフレーム「df_receipt」から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上数量（quantity）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上または売上数量（quantity）が5以上

```python
```
### | P-007
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 売上金額（amount）が1,000以上2,000以下

```python
```

### | P-008
レシート明細のデータフレーム（df_receipt）から売上日（sales_ymd）、顧客ID（customer_id）、商品コード（product_cd）、売上金額（amount）の順に列を指定し、以下の条件を満たすデータを抽出せよ。
+ 顧客ID（customer_id）が"CS018205000001"
+ 商品コード（product_cd）が"P071401019"以外

```python
```

### | P-00
```Python
```

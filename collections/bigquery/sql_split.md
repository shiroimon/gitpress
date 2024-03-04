---
date   : 2022-05-14
title  : 🔍 SPLIT関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "split"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || SPLIT()

```sql
select
    -- SPLITで文字列を' '(スペース)で区切り、offsetで値にアクセス
　    split("hello world everyone", ' ')[offset(0)] -- 0番目の要素　hello
    , split("hello world everyone", ' ')[offset(1)] -- 1番目の要素　world
    , split("hello world everyone", ' ')[offset(2)] -- 2番目の要素　everyone
```

### | アクセス方法
- [OFFSET(0)]：で分割した値にアクセスできる。（０index）
- [ORDINAL(1)]：で分割した値にアクセスできる。（1 index）

```sql
select
　    split("hello world everyone", ' ')[offset(0)] -- 0番目の要素　hello
    , split("hello world everyone", ' ')[ordinal(1)] -- 1番目の要素　hello
```

※ SPLIT関数の戻り値はARRAY型の為、特定配列にアクセスする場合。
- [SAFE_OFFSET(0)]：0を始めとする.
- [SAFE_ORDINAL(1)]：1を始めとする.

```sql
select 
      oaza_name_kana
    , split(oaza_name_kana, ' ')[offset(0)] as oaza_name_kana0 -- 大字
    
    -- , nth(2, split(oaza_name_kana, ' ')) as oaza_name_kana1 -- NTH関数がレガシーSQLの為使用不可
    , split(oaza_name_kana, ' ')[safe_offset(1)] as oaza_name_kana1 -- 丁目
from 
    jp_addresses
```


## || REFERENCE
+ [SPLIT](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions?hl=ja#split) - GoogleCloud 
+ [[bigquery]項目を指定した文字列で分割する方法](https://apl-py.com/tec/bigquery%E9%A0%85%E7%9B%AE%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%9F%E6%96%87%E5%AD%97%E5%88%97%E3%81%A7%E5%88%86%E5%89%B2%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95) - 目黒で働く分析担当の作業メモ
+ [BigQuery：SPLIT（）は1つの値のみを返します](https://www.web-dev-qa-db-ja.com/ja/google-bigquery/bigquery%EF%BC%9Asplit%EF%BC%88%EF%BC%89%E3%81%AF1%E3%81%A4%E3%81%AE%E5%80%A4%E3%81%AE%E3%81%BF%E3%82%92%E8%BF%94%E3%81%97%E3%81%BE%E3%81%99/1050712512/) - web-dev-qa-db-ja.com
+ [BigQueryで文字列の特定文字以前・以降を切り出して結合する](https://ten-ezo.com/9f3a3c688f214588910558193cbb11fb) - TEN's life
+ [BigQuery　レガシーSQLのNTH関数を標準SQLで記述する](https://qiita.com/rik-83/items/557c6f8e9f19fb4af223) - Qiita

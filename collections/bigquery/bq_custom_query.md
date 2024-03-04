---
date    : 2022-01-01
title   : 🔍 カスタムクエリ
excerpt : ---
tags    : ["🔍", "Google BigQuery", "カスタムクエリ"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || カスタムクエリ

|パラメータ|用途|
|:-:|:-:|
|@DS_USER_EMAIL|ログイン ユーザーのメールアドレスを取得します。|

```SQL
-- メール パラメータの例:
select 
    * 
from 
    Sales 
where 
    sales-rep-email = @DS_USER_EMAIL
;
```

### | 
```SQL
#standardSQL
SELECT
  word,
  word_count
FROM
  `bigquery-public-data.samples.shakespeare`
WHERE
  corpus = @corpus -- 
  AND word_count >= @min_word_count -- 
ORDER BY
  word_count DESC;
```



## || REFERENCE
+ [カスタムクエリでパラメータを使用する](https://support.google.com/datastudio/answer/10588439?hl=ja) - Dataportal Help
+ [パラメータ](https://support.google.com/datastudio/answer/9002005#zippy=%2C%E3%81%93%E3%81%AE%E8%A8%98%E4%BA%8B%E3%81%AE%E5%86%85%E5%AE%B9) - Dataportal Help
+ [パラメータ化されたクエリの実行](https://cloud.google.com/bigquery/docs/parameterized-queries) - Google Cloud

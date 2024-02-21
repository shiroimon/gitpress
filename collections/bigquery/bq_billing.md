---
date    : 2024-01-01
title   : 🔍 Billing
excerpt : ---
tags    : ["🔍", "BigQuery", "GoogleCloud"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)


## || MOTIVATION

毎月高くね？会社が払うからいいか〜... <br>
って、なってたがそれじゃいけないと一応使い過ぎ無いようにしたい。



## || 料金体系
#### Computing(分析)料金
|Billing                      |Fee            |Free  |
|:-|-:|-:|
|1. Query(オンデマンド)       |$7.50/TB       |1TB/mm|
|2. Query(月定額)             |$2,400/100slots|-     |
|3. Query(年定額)             |$2.040/100slots|-     |
|4. BQeditions(Standard)      |$0.051/slots(h)|-     |
|5. BQeditions(Enterprise)    |$0.076/slots(h)|-     |
|6. BQeditions(EnterprisePlus)|$0.128/slots(h)|-     |

#### Strage料金
|Billing           |Fee      |Free   |
|:-|-:|-:|
|1. Activ    (論理)|$0.023/GB|10GB/mm|
|2. Longterm (論理)|$0.016/GB|10GB/mm|
|3. Active   (物理)|$0.052/GB|10GB/mm|
|4. Longterm (物理)|$0.026/GB|10GB/mm|

cf. 
- [費用の見積もりと管理](https://cloud.google.com/bigquery/docs/best-practices-costs?hl=ja)-GoogleCloud
- [BigQuery の料金](https://cloud.google.com/bigquery/pricing?hl=ja) -Google Cloud
- [Google BigQuery の料金体系を解説](https://www.dsk-cloud.com/blog/bigquery-pricing-and-points)-電算ｼｽﾃﾑ
- [BigQuery エディションの概要](https://cloud.google.com/bigquery/docs/editions-intro?hl=ja)-GoogleCloud



## || 見積もり
#### 計算式

    = BQ格納データサイズ / 4 * 単価

- BQ格納データサイズ
- 圧縮率 (上記では`25%`と保守的に想定)
- [単価](https://cloud.google.com/bigquery/pricing?hl=en#storage)

cf. 
- [Welcome to Google Cloud's pricing calculator](https://cloud.google.com/products/calculator?hl=ja)


#### dry run

```python
# e.g.

from google.cloud import bigquery

# Construct a BigQuery client object.
client = bigquery.Client()

job_config = bigquery.QueryJobConfig(dry_run=True, use_query_cache=False)

# Start the query, passing in the extra configuration.
query_job = client.query(
    (
        "SELECT name, COUNT(*) as name_count "
        "FROM `bigquery-public-data.usa_names.usa_1910_2013` "
        "WHERE state = 'WA' "
        "GROUP BY name"
    ),
    job_config=job_config,
)  # Make an API request.

# A dry run query completes immediately.
print("This query will process {} bytes.".format(query_job.total_bytes_processed))
```

cf. 
- [クエリ費用を管理する](https://cloud.google.com/bigquery/docs/best-practices-costs?hl=ja#control-query-cost)

#### INFORMATION_SCHEMA.TABLE_STORAGE_BY_ORGANIZATION
```sql
DECLARE active_logical_pricing, longterm_logical_pricing, active_physical_pricing, longterm_physical_pricing NUMERIC;
SET active_logical_pricing    = 0.023;
SET longterm_logical_pricing  = 0.016;
SET active_physical_pricing   = 0.052;
SET longterm_physical_pricing = 0.026;
-- 上記は2023/07時点の東京リージョン価格

SELECT
    project_id
    , ROUND(SUM(total_logical_bytes/POW(1024, 3)), 2) AS total_logical_gigabytes
    , ROUND(SUM(total_physical_bytes/POW(1024, 3)), 2) AS total_physical_gigabytes
    , ROUND(SUM(total_logical_bytes)/SUM(total_physical_bytes), 2) AS compression_ratio
    , ROUND(SUM((active_logical_bytes/POW(1024, 3)) * active_logical_pricing + (long_term_logical_bytes/POW(1024, 3)) * longterm_logical_pricing), 2) AS total_logical_prices
    , ROUND(SUM((active_physical_bytes/POW(1024, 3)) * active_physical_pricing + (long_term_physical_bytes/POW(1024, 3))* longterm_physical_pricing), 2) AS total_active_prices
FROM
    -- データセットリージョンに合わせて変更
    region-asia-northeast1.INFORMATION_SCHEMA.TABLE_STORAGE_BY_ORGANIZATION
GROUP BY 
    project_id
```

cf. [BigQueryの料金体系(BigQuery Editions)を徹底解説 > INFORMATION_SCHEMA へのクエリ](https://blog.g-gen.co.jp/entry/bigquery-editions-explained#INFORMATION_SCHEMA-%E3%81%B8%E3%81%AE%E3%82%AF%E3%82%A8%E3%83%AA)


## || GA4だとどうなん

|月額料金/サイト例|小規模メディア|中規模ポータル|大規模 Web&App|
|:-|-:|-:|-:|
|月間event     |1,000万 |7,000 万|3億     |
|初月          |¥ 130 　|¥ 1,000 |¥ 4,500 |
|1年後         |¥ 400   |¥ 3,000 |¥ 13,000|
|5年後         |¥ 1,400 |¥ 10,000|¥ 44,000|

- $1.0   = ¥130 
- 1.0GB  = 60万event [cf.](https://support.google.com/analytics/answer/9358801?hl=ja&utm_id=ad)

cf. 
- [GA4 の BigQuery エクスポート料金はいくら？目安と料金を抑える方法について](https://www.convpath.com/blog/ga4-bigquery-export/)-convpath
- [GoogleBigQueryは無料？ 課金形態とコスト管理方法の紹介](https://note.com/calm_laelia838/n/nbe54e27dfa87)-note



## || 節約術
#### Physical storage 移行
#### select * 
#### Clustering 
クラスタリングでクエリ費用を抑える
#### Partitioning (e.g. TABLE_SUFFIX)
パーティショニングよりクエリ費用を抑える


---
date    : 2024-01-01
title   : 🔍 error
excerpt : cross region
tags    : ["🔍", "BigQuery", "GoogleCloud", "error"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || 
### | ⛑️ 「別プロジェクトのデータ使うなら同じリージョンでせい！ｺﾗｰ」
```txt
To copy a table, the destination and source datasets must be in the same region. 
Copy an entire dataset to move data between regions.
```

作成したクエリの抽出結果（300万レコード程）を一時的にテーブル化しようとしたら、上記のようなエラーが出て困った。。


似たような事象の人が相談していた。

cf. [Location error on BigQuery](https://www.googlecloudcommunity.com/gc/Data-Analytics/Location-error-on-BigQuery/m-p/424261) -GoogleCloud



## || REFERENCE
- [クロスリージョン データセット レプリケーション](https://cloud.google.com/bigquery/docs/data-replication?hl=ja) -GoogleCloud
- [Google BigQuery はロケーションをまたいだクエリが書けない](https://note.com/miya_y/n/nc18b0a6e1063) -notei
- [BigQueryのクロスリージョン・データセットレプリケーションを解説](https://blog.g-gen.co.jp/entry/using-bigquery-cross-region-replication) -G gen
- [BigQueryのCloudSQL連携クエリを試してみました。](https://techblog.gmo-ap.jp/2022/12/24/bigquery-cloudsql-connection/) -Techblog by GMO

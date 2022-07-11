---
date    : 2022-01-01
title   : __TABLE__
excerpt : 
tags    : ["Google BigQuery", ""]
---
## ||__TABLE__ メタテーブルからの取得
```sql
select * from `dataset.__TABLE__`; 
```

> __TABLES__を利用すると、bq showにも匹敵する情報を全テーブル一括で取得することが出来ます。
> 取れる情報は「テーブル作成日」「テーブル最終更新日」「テーブル行数」「テーブルバイト数」です。
> 
> GMO*


## || REFERENCE
- [INFORMATION_SCHEMA を使用したテーブル メタデータの取得](https://cloud.google.com/bigquery/docs/information-schema-tables?hl=ja) - GoogleCloud
- [公式ドキュメントにない方法で、BigQueryにあるデータセット内の全てのテーブル・ビューの情報を取得してみた](https://a7xche.hatenablog.com/entry/2020/09/12/222045) -Hatena
- [BigQueryで全テーブルのメタ情報を一括で取得する方法](https://techblog.gmo-ap.jp/2019/12/25/bigquery_table_meta_info/) - GMO*

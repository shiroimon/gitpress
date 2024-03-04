---
date    : 2023-03-10
title   : 🔍 クエリパラメータ
excerpt : ---
tags    : ["🔍", "Google BigQuery", "クエリパラメータ"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || クエリパラメータ（クエリストリング）

## || BigQueryで使う
### | CLI (基本コッチでしか使えないみたい...)
```shell
$ bq query --parameter='param::A' 'select @param'
Waiting on bqjob_xxxx ... (0s) Current status: DONE
+-----+
| f0_ |
+-----+
| A   |
+-----+
```
cf.[パラメータ化されたクエリの実行](https://cloud.google.com/bigquery/docs/parameterized-queries?hl=ja) - GoogleCloud

### | WebUI
```SQL
execute immediate """
    select @param
"""
using
1 as param
```
動的SQLにすれば使える。（execute immediate配下で使えるが、文字列になるから見にくい）

cf. 
- [BigQueryクエリパラメータまとめ](https://qiita.com/damassima/items/899c00935594b60c4020) - Qiita
- [EXECUTE IMMEDIATE](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting?hl=ja#execute_immediate) - GoogleCloud


(関連) [🔍 GA4　構造化データをフラット化](https://gitpress.io/c/bigquery/ga_ga4_get_flaten)



## || REFERENCE
- [クエリパラメータ](https://cloud.google.com/bigquery/docs/reference/standard-sql/lexical?hl=ja#query_parameters) - GoogleCloud
- [パラメータ化されたクエリの実行](https://cloud.google.com/bigquery/docs/parameterized-queries?hl=ja) - GoogleCloud
- [EXECUTE IMMEDIATE](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting?hl=ja#execute_immediate) - GoogleCloud
- [BigQueryクエリパラメータまとめ](https://qiita.com/damassima/items/899c00935594b60c4020) - Qiita
- [パスパラメータとクエリパラメータの違い](https://zenn.dev/eri_agri/articles/859a3362db8386) - Zenn
- [URL クエリパラメータの正しい設定方法](https://help.webantenna.info/8821/) - WebAntenna活用ノート
- [クエリストリング（クエリ文字列・URLパラメータ）とは何か？](https://ssaits.jp/promapedia/technology/query-string.html) - Promapedia

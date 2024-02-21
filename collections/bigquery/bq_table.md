---
date    : 2022-01-01
title   : 🔍 疑似中間テーブル作成
excerpt : ---
tags    : ["🔍", "Google BigQuery", ""]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || よく出るコイツ
```
Resources exceeded during query execution: Not enough resources for query planning - too many subqueries or query is too complex
```

> WITH句を多用していたり、ビューの多段が10個近くになってくるとこのエラーがよく発生することが多いです*2。

と、参考にしたすごい人も言っている。



## || 解決策！疑似的中間テーブル作成
### | テンポラリーテーブル作成
```SQL
CREATE TEMP TABLE {テーブル名} ({処理});
```

### | クエリ
```SQL
-- EXTRACTED中間テーブル作成（疑似的入れ物）
create temp table EXTRACTED (
    COMMIT string
    , TREE string
);

-- 中間テーブルに入れ込むデータ
insert into EXTRACTED
select COMMIT, TREE from `bigquery-public-data.github_repos.sample_commits` limit 10;

-- 中間テーブルにアクセス
select * from EXTRACTED;
```

### | うれしいこと
+ サブクエリ多様で、処理落ちすることを回避できる。（エラー回避）
+ エラー回避で、都度中間テーブルとして保存していたデータを作成しなくて済む。（データプランク化回避）
    - 一時テーブルであるため、BigQueryが削除をやってくれる。
      作っては後で消して、を手作業でしていた。（消し忘れて上司に「ゴミはゴミ箱へ！」とよく言われたもの...) 



## || REFERENCE
+ [BigQueryのQuery is too complexにもう悩まされたくない](https://qiita.com/pakio/items/fdb4003ae6b6c10320b3) - Qiita
+ [BigQuery Scriptingの実例: 一時テーブルを使ってtoo many subqueries or query is too complexを回避する](https://www.yasuhisay.info/entry/2022/03/14/093500#%E4%BE%8B-%E4%B8%80%E6%99%82%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6too-many-subqueries-or-query-is-too-complex%E3%82%92%E5%9B%9E%E9%81%BF%E3%81%99%E3%82%8B) - yasuhisa's blog 

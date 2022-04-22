---
date   : 2022-03-02
title  : UDF
excerpt: ユーザー定義関数
tags   : ["Google BigQuery", "SQL", "UDF", "自作関数", "TEMPORARY FUNCTION"]
---

## || ユーザー定義関数（UDF）
詳細は、Cf.へ

### | メリット
1. コーディングのコストを低減できる
2. コードを理解しやすくできる
3. バグ混入のリスクを低減できる

### | コーディング
```sql
------------------------------------ユーザー定義関数------------------------------------
-- visitStartTimeを日本時間の日付に変更する関数
CREATE TEMPORARY FUNCTION DATE_BY_VISITSTARTTIME(visitStartTime INT64)
    AS (
        DATE(TIMESTAMP_SECONDS(visitStartTime), "Asia/Tokyo"))
    );

-- ページに対応するIDを返す関数 --★ 複数回使っているUDFだが、この処理を変更すると全ての処理に変更が適応される
CREATE TEMPORARY FUNCTION PAGECATEGORY_ID_BY_PAGECATEGORY_NAME(pageCategoryName STRING)
    AS(
        CASE pageCategoryName
            WHEN "トップ" THEN 1
            WHEN "検索結果" THEN 2
            WHEN "商品詳細" THEN 3
            WHEN "その他" THEN 4
        END
    );

-- ページを判定し対応するページIDを返す関数
CREATE TEMPORARY FUNCTION CATEGORIZE_WEB_PAGECATEGORY_ID(pagePath STRING)
    AS (
        CASE
            WHEN REGEXP_CONTAINS(pagePath, r"[トップのページパスに該当する正規表現]")
                THEN PAGECATEGORY_ID_BY_PAGECATEGORY_NAME("トップ")
            WHEN REGEXP_CONTAINS(pagePath, r"[検索結果のページパスに該当する正規表現]")
                THEN PAGECATEGORY_ID_BY_PAGECATEGORY_NAME("検索結果")
            WHEN REGEXP_CONTAINS(pagePath, r"[商品詳細のページパスに該当する正規表現]")
                THEN PAGECATEGORY_ID_BY_PAGECATEGORY_NAME("商品詳細")
            ELSE
                PAGECATEGORY_ID_BY_PAGECATEGORY_NAME("その他")
        END
    );

--★ ここまではチーム内で共通して使っているUDFなので、レビュー不要
 
SELECT 
    DATE_BY_VISITSTARTTIME(visitStartTime) AS visitDate --★ 日時の変換処理が直感的でバグが混入しづらい
    , CATEGORIZE_WEB_PAGECATEGORY_ID(hits.page.pagePath) AS pageCategoryId --★ 記述すると長くなる分類処理が短く済む
    , COUNT(DISTINCT fullVisitorId) AS UU
FROM
    `[セッション情報からなるテーブル]`, UNNEST(hits) AS hits
WHERE
    pageCategoryId <> PAGECATEGORY_ID_BY_PAGECATEGORY_NAME("その他") --★ <> 4 だと4の意味がわからない、誤りがあっても気づけない
GROUP BY
    visitDate
    , pageCategoryId
```

> クエリ全体で見ると行数が多く感じるかもしれませんが、書く・読む作業が発生するのはほぼSELECT文のみで行数は少ないです。UDF部分はすでに用意してあるものを使っているため、書く・読む手間はほぼありません。
>
> 今回はUDFを一時的なUDFとして扱っていますが、永続的なUDFとして予め用意していると記述する必要もなくなります。
> 
> 参考：[標準 SQL ユーザー定義関数 | BigQuery | Google Cloud](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions?hl=ja)

（参考になりすぎるTT）


### | まねっこ（自作関数）

```sql
create temporary function 
    CALC_ELAPSED_MONTHS(base_date datetime, value_date datetime) as (
    /*
     * 経過月数を正しく規定する関数
     * eg. 集計期間2011/2/1~2012/1/31の1年間とする時、2011/1/31から翌日に予約をした会員も月差分で加えられてしまう。
     *     回避として、そういった1ケ月満たない会員に「-1」を渡す処理をする。
     */
        date_diff(base_date, value_date, month) 
        + if(date_diff(base_date, date_add(value_date, interval date_diff(base_date, value_date, month) month), day) >= 0, 0, -1)
    );
```

```sql
```


## || Cf.
+ [BigQueryでユーザー定義関数（UDF）は武器になるという話](https://techblog.zozo.com/entry/bigquery-udf) - ZOZO
+ [Standard SQL user-defined functions](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions?hl=ja) - Google Cloud
+ []() -


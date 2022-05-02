
## | UNNEST()

UNNEST を使うと、ARRAYなどの配列や、REPEATEDなカラムを、開くことができる。

```sql
```

```sql 
    # unnest を用いて、親テーブルに無い値を付与
    , search_list_marge as (
        select distinct KEY_CODE, KEY_NAME from `project.datasets.search_table_20*` where _TABLE_SUFFIX = (select TODAY from ts)
        union all
        select * from
            unnest(
                array
                <struct< KEY_CODE STRING, KEY_NAME STRING>>
                [("reserve_tomorrow","明日予約可"), ("reserve_today","今日予約可")]
            )
    )
```


## | cf.
+ [BigqueryでUNNESTを使いこなせ！クエリ効率１００%！！最強！！](https://medium.com/eureka-engineering/bigquery-unnest-100percent-3d28560b4f0)-  medium
+ [BigQuery 活用術: UNNEST 関数](https://developers-jp.googleblog.com/2017/04/bigquery-tip-unnest-function.html)- Google Developers
+ [BigQuery で複数の配列をフラット化する](https://labs.septeni.co.jp/entry/2018/11/06/120000) - FLINTERS Engineer's Blog

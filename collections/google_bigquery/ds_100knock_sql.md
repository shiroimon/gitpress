---
date    : 2021-11-15
title   : 🔍 100本ノック
excerpt : データ確認
tags    : ["🔍", "DataScientist", "SQL", "BigQuery"]
---

## || データサイエンス100本ノック（構造化データ加工編） SQL編

### | 各テーブル確認
```SQL
-- 各テーブルのデータ数確認
select　*　from(
    select
        'category' as tb_name
        , (select count(*) from `prj-test3.100knocks.category`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'category'

    union all

    select
        'customer' as tb_name
        , (select count(*) from `prj-test3.100knocks.customer`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'customer'

    union all

    select
        'geocode' as tb_name
        , (select count(*) from `prj-test3.100knocks.geocode`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'geocode'

    union all

    select
        'product' as tb_name
        , (select count(*) from `prj-test3.100knocks.product`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'product'

    union all

    select
        'receipt' as tb_name
        , (select count(*) from `prj-test3.100knocks.receipt`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'receipt'

    union all

    select
        'store' as tb_name
        , (select count(*) from `prj-test3.100knocks.store`) as reco
        , count(*) as col
    from `prj-test3.100knocks.INFORMATION_SCHEMA.COLUMNS`
    where table_name = 'store'
)
order by reco desc
;
```

![img](https://i.gyazo.com/efee5ae5565e1299e43fe26507b6d738.png)


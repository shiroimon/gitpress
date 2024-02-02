---
date    : 2022-09-26
title   : 🔍 GREATEST関数
excerpt : ---
tags    : ["Google BigQuery", "greatest"]
---

## || greatest()


### | 使用例
```SQL
 --・施設別 × 終業時間
    , SHOP_END_TIME as (
        select 
            * 
            , greatest(
                    MON_END_TIME, MON_END_TIME2
                  , TUE_END_TIME, TUE_END_TIME2
                  , WED_END_TIME, WED_END_TIME2
                  , THU_END_TIME, THU_END_TIME2
                  , FRI_END_TIME, FRI_END_TIME2
                  , SAT_END_TIME, SAT_END_TIME2
                  , SUN_END_TIME, SUN_END_TIME2
                  , HOL_END_TIME, HOL_END_TIME2
              ) as END_TIME
        from (
            select 
                CATALOG_ID
                , ifnull(if(MON_END_TIME = 0, 2400, MON_END_TIME ), 0) as MON_END_TIME
                , ifnull(if(MON_END_TIME2= 0, 2400, MON_END_TIME2), 0) as MON_END_TIME2
                , ifnull(if(TUE_END_TIME = 0, 2400, TUE_END_TIME ), 0) as TUE_END_TIME
                , ifnull(if(TUE_END_TIME2= 0, 2400, TUE_END_TIME2), 0) as TUE_END_TIME2
                , ifnull(if(WED_END_TIME = 0, 2400, WED_END_TIME ), 0) as WED_END_TIME
                , ifnull(if(WED_END_TIME2= 0, 2400, WED_END_TIME2), 0) as WED_END_TIME2
                , ifnull(if(THU_END_TIME = 0, 2400, THU_END_TIME ), 0) as THU_END_TIME
                , ifnull(if(THU_END_TIME2= 0, 2400, THU_END_TIME2), 0) as THU_END_TIME2
                , ifnull(if(FRI_END_TIME = 0, 2400, FRI_END_TIME ), 0) as FRI_END_TIME
                , ifnull(if(FRI_END_TIME2= 0, 2400, FRI_END_TIME2), 0) as FRI_END_TIME2
                , ifnull(if(SAT_END_TIME = 0, 2400, SAT_END_TIME ), 0) as SAT_END_TIME
                , ifnull(if(SAT_END_TIME2= 0, 2400, SAT_END_TIME2), 0) as SAT_END_TIME2
                , ifnull(if(SUN_END_TIME = 0, 2400, SUN_END_TIME ), 0) as SUN_END_TIME
                , ifnull(if(SUN_END_TIME2= 0, 2400, SUN_END_TIME2), 0) as SUN_END_TIME2
                , ifnull(if(HOL_END_TIME = 0, 2400, HOL_END_TIME ), 0) as HOL_END_TIME
                , ifnull(if(HOL_END_TIME2= 0, 2400, HOL_END_TIME2), 0) as HOL_END_TIME2
            from (
                select 
                    CATALOG_ID
                    , safe_cast(replace(MON_END_TIME , ':', '') as int64) as MON_END_TIME
                    , safe_cast(replace(MON_END_TIME2, ':', '') as int64) as MON_END_TIME2
                    , safe_cast(replace(TUE_END_TIME , ':', '') as int64) as TUE_END_TIME
                    , safe_cast(replace(TUE_END_TIME2, ':', '') as int64) as TUE_END_TIME2
                    , safe_cast(replace(WED_END_TIME , ':', '') as int64) as WED_END_TIME
                    , safe_cast(replace(WED_END_TIME2, ':', '') as int64) as WED_END_TIME2
                    , safe_cast(replace(THU_END_TIME , ':', '') as int64) as THU_END_TIME
                    , safe_cast(replace(THU_END_TIME2, ':', '') as int64) as THU_END_TIME2
                    , safe_cast(replace(FRI_END_TIME , ':', '') as int64) as FRI_END_TIME
                    , safe_cast(replace(FRI_END_TIME2, ':', '') as int64) as FRI_END_TIME2
                    , safe_cast(replace(SAT_END_TIME , ':', '') as int64) as SAT_END_TIME
                    , safe_cast(replace(SAT_END_TIME2, ':', '') as int64) as SAT_END_TIME2
                    , safe_cast(replace(SUN_END_TIME , ':', '') as int64) as SUN_END_TIME
                    , safe_cast(replace(SUN_END_TIME2, ':', '') as int64) as SUN_END_TIME2
                    , safe_cast(replace(HOL_END_TIME , ':', '') as int64) as HOL_END_TIME
                    , safe_cast(replace(HOL_END_TIME2, ':', '') as int64) as HOL_END_TIME2
                from SHOP_DNT 
            )
        )
    ) 

```

## || REFERENCE
* [GREATEST](https://cloud.google.com/bigquery/docs/reference/standard-sql/mathematical_functions?hl=ja#greatest) - GoogleCloud
* [照合について](https://cloud.google.com/bigquery/docs/reference/standard-sql/collation-concepts?hl=ja#collate_about) - GoogleClod
* []() - 

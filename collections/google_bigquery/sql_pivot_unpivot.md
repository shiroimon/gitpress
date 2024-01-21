---
date    : 2022-11-15
title   : 🔍 PIVOT / UNPIVOT演算子
excerpt : ---
tags    : ["Google BigQuery", "pivot", "unpivot", "横持ち変換"]
---

## || PIVOT演算子
```sql
from 
    TABLE
pivot(
    [集計関数]
    for [カラム名]
    in ([値：in句の利用])
) as [エイリアス]
```
PIVOT 演算子は、集計を使用して行を列に変換できる。PIVOT は FROM 句の一部。


### | 縦持ちデータ→横持ちに変換
年月別に集計をしたい。
```SQL
# ~ 上述のWITH句は割愛
    -- 縦持ち 
    , RAWDATA as(
        select
            CATALOG_ID
            , CUST_NO
            , CLINIC_NAME
            , SERVICE_START_DATE
            , SERVICE_END_DATE
            , format_date("%Y%m", LOG_MONTH) as LOG_MONTH -年月(pivot用にstring化)
            , PV_CNT --PV数
            , UU_CNT --UU数
            , CV1    --①webのみ（※HPやGMB除外）
            , CV2    --②webのみ（※HPやGMBも含む）
            , CV3    --③web＋PPC予約（※HPやGMB除外）
            , CV4    --④web＋PPC予約＋院内予約（※HPやGMB除外）
        from 
            T_UNITE
        where 
            LOG_MONTH between date("2017-01-01") and date("2022-10-01") --指定期間
    )
    
    -- 横持ち
    , REPORT as (
        select 
            CATALOG_ID           --医院ID
            , CUST_NO            --顧客番号
            , CLINIC_NAME        --医院名
            , SERVICE_START_DATE --契約開始年月日
            , SERVICE_END_DATE   --契約終了年月日
        --PV
        , ifnull(PV_201901, 0) as PV_201901, ifnull(PV_201902, 0) as PV_201902, ifnull(PV_201903, 0) as PV_201903, ifnull(PV_201904, 0) as PV_201904, ifnull(PV_201905, 0) as PV_201905, ifnull(PV_201906, 0) as PV_201906, ifnull(PV_201907, 0) as PV_201907, ifnull(PV_201908, 0) as PV_201908, ifnull(PV_201909, 0) as PV_201909, ifnull(PV_201910, 0) as PV_201910, ifnull(PV_201911, 0) as PV_201911, ifnull(PV_201912, 0) as PV_201912
        , ifnull(PV_202001, 0) as PV_202001, ifnull(PV_202002, 0) as PV_202002, ifnull(PV_202003, 0) as PV_202003, ifnull(PV_202004, 0) as PV_202004, ifnull(PV_202005, 0) as PV_202005, ifnull(PV_202006, 0) as PV_202006, ifnull(PV_202007, 0) as PV_202007, ifnull(PV_202008, 0) as PV_202008, ifnull(PV_202009, 0) as PV_202009, ifnull(PV_202010, 0) as PV_202010, ifnull(PV_202011, 0) as PV_202011, ifnull(PV_202012, 0) as PV_202012
        , ifnull(PV_202101, 0) as PV_202101, ifnull(PV_202102, 0) as PV_202102, ifnull(PV_202103, 0) as PV_202103, ifnull(PV_202104, 0) as PV_202104, ifnull(PV_202105, 0) as PV_202105, ifnull(PV_202106, 0) as PV_202106, ifnull(PV_202107, 0) as PV_202107, ifnull(PV_202108, 0) as PV_202108, ifnull(PV_202109, 0) as PV_202109, ifnull(PV_202110, 0) as PV_202110, ifnull(PV_202111, 0) as PV_202111, ifnull(PV_202112, 0) as PV_202112
        , ifnull(PV_202201, 0) as PV_202201, ifnull(PV_202202, 0) as PV_202202, ifnull(PV_202203, 0) as PV_202203, ifnull(PV_202204, 0) as PV_202204, ifnull(PV_202205, 0) as PV_202205, ifnull(PV_202206, 0) as PV_202206, ifnull(PV_202207, 0) as PV_202207, ifnull(PV_202208, 0) as PV_202208, ifnull(PV_202209, 0) as PV_202209, ifnull(PV_202210, 0) as PV_202210
        -- UU
        , ifnull(UU_201901, 0) as UU_201901, ifnull(UU_201902, 0) as UU_201902, ifnull(UU_201903, 0) as UU_201903, ifnull(UU_201904, 0) as UU_201904, ifnull(UU_201905, 0) as UU_201905, ifnull(UU_201906, 0) as UU_201906, ifnull(UU_201907, 0) as UU_201907, ifnull(UU_201908, 0) as UU_201908, ifnull(UU_201909, 0) as UU_201909, ifnull(UU_201910, 0) as UU_201910, ifnull(UU_201911, 0) as UU_201911, ifnull(UU_201912, 0) as UU_201912
        , ifnull(UU_202001, 0) as UU_202001, ifnull(UU_202002, 0) as UU_202002, ifnull(UU_202003, 0) as UU_202003, ifnull(UU_202004, 0) as UU_202004, ifnull(UU_202005, 0) as UU_202005, ifnull(UU_202006, 0) as UU_202006, ifnull(UU_202007, 0) as UU_202007, ifnull(UU_202008, 0) as UU_202008, ifnull(UU_202009, 0) as UU_202009, ifnull(UU_202010, 0) as UU_202010, ifnull(UU_202011, 0) as UU_202011, ifnull(UU_202012, 0) as UU_202012
        , ifnull(UU_202101, 0) as UU_202101, ifnull(UU_202102, 0) as UU_202102, ifnull(UU_202103, 0) as UU_202103, ifnull(UU_202104, 0) as UU_202104, ifnull(UU_202105, 0) as UU_202105, ifnull(UU_202106, 0) as UU_202106, ifnull(UU_202107, 0) as UU_202107, ifnull(UU_202108, 0) as UU_202108, ifnull(UU_202109, 0) as UU_202109, ifnull(UU_202110, 0) as UU_202110, ifnull(UU_202111, 0) as UU_202111, ifnull(UU_202112, 0) as UU_202112
        , ifnull(UU_202201, 0) as UU_202201, ifnull(UU_202202, 0) as UU_202202, ifnull(UU_202203, 0) as UU_202203, ifnull(UU_202204, 0) as UU_202204, ifnull(UU_202205, 0) as UU_202205, ifnull(UU_202206, 0) as UU_202206, ifnull(UU_202207, 0) as UU_202207, ifnull(UU_202208, 0) as UU_202208, ifnull(UU_202209, 0) as UU_202209, ifnull(UU_202210, 0) as UU_202210
        -- CV1
        , ifnull(CV1_201901, 0) as CV1_201901, ifnull(CV1_201902, 0) as CV1_201902, ifnull(CV1_201903, 0) as CV1_201903, ifnull(CV1_201904, 0) as CV1_201904, ifnull(CV1_201905, 0) as CV1_201905, ifnull(CV1_201906, 0) as CV1_201906, ifnull(CV1_201907, 0) as CV1_201907, ifnull(CV1_201908, 0) as CV1_201908, ifnull(CV1_201909, 0) as CV1_201909, ifnull(CV1_201910, 0) as CV1_201910, ifnull(CV1_201911, 0) as CV1_201911, ifnull(CV1_201912, 0) as CV1_201912
        , ifnull(CV1_202001, 0) as CV1_202001, ifnull(CV1_202002, 0) as CV1_202002, ifnull(CV1_202003, 0) as CV1_202003, ifnull(CV1_202004, 0) as CV1_202004, ifnull(CV1_202005, 0) as CV1_202005, ifnull(CV1_202006, 0) as CV1_202006, ifnull(CV1_202007, 0) as CV1_202007, ifnull(CV1_202008, 0) as CV1_202008, ifnull(CV1_202009, 0) as CV1_202009, ifnull(CV1_202010, 0) as CV1_202010, ifnull(CV1_202011, 0) as CV1_202011, ifnull(CV1_202012, 0) as CV1_202012
        , ifnull(CV1_202101, 0) as CV1_202101, ifnull(CV1_202102, 0) as CV1_202102, ifnull(CV1_202103, 0) as CV1_202103, ifnull(CV1_202104, 0) as CV1_202104, ifnull(CV1_202105, 0) as CV1_202105, ifnull(CV1_202106, 0) as CV1_202106, ifnull(CV1_202107, 0) as CV1_202107, ifnull(CV1_202108, 0) as CV1_202108, ifnull(CV1_202109, 0) as CV1_202109, ifnull(CV1_202110, 0) as CV1_202110, ifnull(CV1_202111, 0) as CV1_202111, ifnull(CV1_202112, 0) as CV1_202112
        , ifnull(CV1_202201, 0) as CV1_202201, ifnull(CV1_202202, 0) as CV1_202202, ifnull(CV1_202203, 0) as CV1_202203, ifnull(CV1_202204, 0) as CV1_202204, ifnull(CV1_202205, 0) as CV1_202205, ifnull(CV1_202206, 0) as CV1_202206, ifnull(CV1_202207, 0) as CV1_202207, ifnull(CV1_202208, 0) as CV1_202208, ifnull(CV1_202209, 0) as CV1_202209, ifnull(CV1_202210, 0) as CV1_202210
        from 
            RAWDATA
        pivot (
              sum(PV_CNT) as PV
            , sum(UU_CNT) as UU
            , sum(CV1) as CV1
            , sum(CV2) as CV2
            , sum(CV3) as CV3
            , sum(CV4) as CV4
            for LOG_MONTH in (
                "201901","201902","201903","201904","201905","201906","201907","201908","201909","201910","201911","201912",
                "202001","202002","202003","202004","202005","202006","202007","202008","202009","202010","202011","202012",
                "202101","202102","202103","202104","202105","202106","202107","202108","202109","202110","202111","202112",
                "202201","202202","202203","202204","202205","202206","202207","202208","202209","202210"
            ) 
        ) -- pivot
    )
 select * from REPORT;

```


## || UNPIVOT演算子



## || REFERENCE
- [PIVOT演算子](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax?hl=ja#pivot_operator) - GoogleCloud 
- [UNPIVOT演算子](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax?hl=ja#unpivot_operator) - GoogleCloud 
- [BigQueryでpivotとunpivotするやつ2](https://qiita.com/taniyam/items/cb121edef1b594cd9042) - Qiita
- [BigQueryでpivotとunpivotするやつ](https://qiita.com/taniyam/items/aa235248859499f1bfbb) - Qiita
- [SQL ServerのPIVOT句・UNPIVOT句](https://www.casleyconsulting.co.jp/blog/engineer/162/) - CasleyConsulting
- [BigQueryでpivotを使う](https://zenn.dev/verno3632/articles/8022a6a4bed038) - Zenn

---
date   : 2022-03-14
title  : 🔍 PERCENTAILE_CONT関数 / PERCENTAIL_DISC関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "window関数", "percentile_cont()", "percentile_disc()"]
---

## || 分析関数

```sql
percentile_[cont|disc](percent) 
within group(order by expression [(asc)|desc])
over(partition by partition_list)
```

```sql
#　e.g.
select 
    percentile_cont(0.5) within group(order by val) as median
    percentile_cont(val, 0.5) over() as median_1
```

> PERCENTILE_CONT関数とPERCENTILE_DISC関数は同様の機能をもつ関数ですが、指定されたパーセント値が行と行の間に位置する場合に、動作が異なります。
> 
> PERCENTILE_CONT関数は前後の行で補間された値が返り、PERCENTILE_DISC関数は、指定されたパーセント値を超える行の中で、最小値となる値が返ります。
> 
> たとえば、行の値がそれぞれ、1、2、3、4で、50%の値を取得しようとした場合、50%は2と3の間になります。この場合、各関数の戻り値は次のとおり異なります。
> 
> PERCENTILE_CONT関数
> 
> 2と3（前後の行）でデータが補間されて2.5が返ります。
> 
> PERCENTILE_DISC関数
> 
> 指定したパーセント値を超える行の中で、最小値となる行の値3が返ります。
> 
> Cf. [PERCENTILE_CONT関数とPERCENTILE_DISC関数の違い](https://cs.wingarc.com/manual/drsum/5.5/ja/UUID-aba632ca-4a60-7689-1cef-7a5e8ae4a0f0.html#UUID-aba632ca-4a60-7689-1cef-7a5e8ae4a0f0_d40785e108022) - Dr.Sum

### | percentile_disc()
整数値を返したい時。

### | percentile_cont()
小数値を返す。（対象が偶数値だと線形補完する処理が施されてるとのこと。）



## || Cf.
+ [【BigQuery】standardSQLでパーセンタイル（や中央値）を集計関数のように計算する](https://qiita.com/dr666m1/items/74a921cf6493169e466c) - Qiita
+ [BigQueryで平均、中央値、最頻値を計算する](https://ichi.pro/bigquery-de-heikin-chuochi-sai-shiki-ne-o-keisansuru-243232206520793) - ICHI.PRO
+ [GoogleCloudPlatform/bigquery-utils](https://github.com/GoogleCloudPlatform/bigquery-utils/tree/master/udfs/community) - Github
+ [percentile_disc vs percentile_cont](https://stackoverflow.com/questions/23585667/percentile-disc-vs-percentile-cont) - stack overflow
+ [6-1-13　PERCENTILE_CONT（パーセント値に相当する、補間された値を取得する）](https://cs.wingarc.com/manual/drsum/5.5/ja/UUID-aba632ca-4a60-7689-1cef-7a5e8ae4a0f0.html) - Dr.Sum

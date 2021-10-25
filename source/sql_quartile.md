---
date    : 2021-10-25
title   : 【🔎SQL】要約統計量
excerpt : SQLでデータを分析するにあたり基礎的なウォーミングアップ。
tags    : ["SQL", "BigQuery", "要約統計量", "平均", "標準偏差", "第一四分位", "第三四分位", "中央値"]
---
## || 要約統計量

|n|mean|std|min|25%|median|75%|max|
|-|-   |-  |-  |-  |-     |-  |-  |


```SQL
select
    count(passenger_count) as n,
    avg(passenger_count) as mean,
    stddev(passenger_count) as std,
    min(passenger_count) as min,
    (select q from(select percentile_cont(passenger_count, 0.25) over() as q from `prj.ds.sample_tb`) group by q) as first_quartile,
    (select q from(select percentile_cont(passenger_count, 0.5) over() as q from `prj.ds.sample_tb`) group by q) as median,
    (select q from(select percentile_cont(passenger_count, 0.75) over() as q from `prj.ds.sample_tb`) group by q) as thrd_quartile,
    max(passenger_count) as max
from `prj.ds.sample_tb`
where passenger_count is not null
;
```

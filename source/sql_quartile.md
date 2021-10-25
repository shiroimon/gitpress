---
date    : 2021-10-25
title   : 【🔎SQL】要約統計量
excerpt : SQLでデータを分析するにあたり基礎的なウォーミングアップ。
tags    : ["SQL", "BigQuery", "要約統計量", "平均", "標準偏差", "第一四分位", "第三四分位", "中央値"]
---
## || 要約統計量
> 要約統計量とは、標本の分布の特徴を代表的に表す統計学上の値であり、統計量の一種。
> 記述統計量、基本統計量、代表値ともいう。 正規分布の場合は、平均と、分散または標準偏差で分布を記述できる。
> 正規分布からのずれを知るためには、尖度や歪度などの高次モーメントから求められる統計量を用いる。
>
> ー [要約統計量](https://ja.wikipedia.org/wiki/%E8%A6%81%E7%B4%84%E7%B5%B1%E8%A8%88%E9%87%8F)- wikipedia

## || SQL で書いてみる

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

---
date    : 2022-01-01
title   : BigQueryMlでハイパラチューニング
excerpt : 
tags    : ["Google BigQuery", "Hyperparameters"]
---
## || BigQueryMlでハイパラチューニング
### | ハイパーパラメーターのチューニング

- （前）チューニング
```SQL
create or repalce model taxi.TOTAL_AMOUNT_MODEL
options (
  model_type='linear_reg'               -- 線形回帰 
  , input_label_cols = ['total_amount'] -- ラベル名
) as
select * from `taxi.sample_data`;
```
- （後）チューニング
```SQL
create or repalce model taxi.TOTAL_AMOUNT_MODEL
options (
  model_type='linear_reg'               -- 線形回帰 
  , input_label_cols = ['total_amount'] -- ラベル名
  , num_trials = 10
  , hparam_tuning_objectives = ['mean_squared_error'] -- 平均二乗誤差 
  , max_parallel_trials = 5
  , enable_global_explain = TRUE
) as
select * from `taxi.sample_data`;
```
[Hyperparameters and Objectives – Hyperparameter tuning for CREATE MODEL statements](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-hyperparameter-tuning#hyperparameters_and_objectives) - GoogleCloud

### | なんでこんなことするの？

推論の制度が向上するから。（予測精度を上げるため。）

### |




## || REFERENCE
+ [BigQuery MLを利用した予測モデル構築〜パラメータチューニングから評価まで〜](https://datumstudio.jp/blog/20211213-introduction-bigqueryml/) - DATUM STUDIO
+ []() - 

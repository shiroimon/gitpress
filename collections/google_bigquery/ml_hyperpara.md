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
  , hparam_tuning_objectives = ['mean_squared_error'] -- 平均二乗誤差(評価指標)
  , max_parallel_trials = 5
  , enable_global_explain = TRUE -- 特徴量寄与度を確認の為（Explainable AI機能を用いる場合）
) as
select * from `taxi.sample_data`;
```

### | なんでこんなことするの？

推論の制度が向上するから。（予測精度を上げるため。）

### | やみくもに足せばいいのか？

cf. [Hyperparameters and Objectives – Hyperparameter tuning for CREATE MODEL statements](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-hyperparameter-tuning#hyperparameters_and_objectives) - GoogleCloud

### | チューニングしたことで幸せなことが
#### ●モデルの評価のしやすさ
`ML.TRIAL_INFO`を利用することで、学習時のトライアル結果の詳細を確認することが出来る。
（「is_optimal」列を確認すると、何回目のトライアルが最適なのか知れる。）
```SQL
-- 何回目のトライアルがよき？
select * from ml.trall_info(model `taxi.TOTAL_AMOUNT_MODEL`);
```
cf. [The ML.TRAINING_INFO function](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-train) - GoogleCloud

#### ●BigQuery Explainable AI 使える
`enable_global_explain` パラメータを TRUE にしていた場合、「BigQuery Explainable AI」が使える。
使うと、特徴毎の寄与度を確認することが出来る。
（ランダムフォレストのフューチャーインポータンス的なことができるのか。）
```SQL
-- どの特徴量がよき？
select * from ml.global_explain(model `taxi.TOTAL_AMOUNT_MODEL`);
```

cf. [BigQuery Explainable AI Overview](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-xai-overview) - GoogleCloud



## || REFERENCE
+ [BigQuery MLを利用した予測モデル構築〜パラメータチューニングから評価まで〜](https://datumstudio.jp/blog/20211213-introduction-bigqueryml/) - DATUM STUDIO
+ []() - 

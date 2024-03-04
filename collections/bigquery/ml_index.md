---
date    : 2022-06-14
title   : 🔍 BigQuery ML
excerpt : ---
tags    : ["Google BigQuery", "BigQuery ML"]
---

![BigQuery](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2020/09/gcp-eyecatch-bigquery_1200x630.png)

## || BigQueryMLでサポートされるモデル
![image](https://user-images.githubusercontent.com/28585421/173488664-3a6221c8-cb19-4d2b-ac11-58a8b40b15dc.png)
([チートシートのDL](https://cloud.google.com/bigquery-ml/images/ml-model-cheatsheet.pdf) - Google)

### | 教師あり
+ 回帰
  - [Linear regression](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create-glm)（線形回帰）
  - [Deep Neural Network](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create-dnn-models)（DNN、回帰）
  - XGBoost（回帰）
  - [AutoML Tables](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create-automl)（回帰）
+ 分類
  - Binary logistic regression（２項分類）
  - Multiclass logistic regression（多項分類）
  - Deep Neural Network（DNN、分類）
  - XGBoost（分類）
  - AutoML Tables（分類）

### | 教師なし
+ クラスタリング
  - [K-means clustering](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create-kmeans)（K 平均法クラスタリング）
+ レコメンデーション
  - Matrix Factorization
+ 時系列予測
  - Time series（ARIMA）


### | モデリング

cf.[The CREATE MODEL statement](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create) - GoogleCloud


## || REFERENCE
+ [BigQuery ML とは](https://cloud.google.com/bigquery-ml/docs/introduction) - Google CLoud
+ [【入門記事】2020年時点のBigQuery MLまとめ](https://dev.classmethod.jp/articles/bigquery-ml-summary-2020/) - DevelopersIO
+ [BigQuery ML でサポートされるモデル](https://cloud.google.com/bigquery-ml/docs/introduction#supported_models_in) - Google Cloud

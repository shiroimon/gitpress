---
date    : 2022-06-14
title   : Google BigQuery ML
excerpt : 
tags    : ["Google BigQuery", "BigQuery ML", "AutoML"]
---

## || BigQueryMLでAutoML
### | データセット収集

[オープンデータ](https://hsi.ksc.kwansei.ac.jp/AI-Center/opendata.html)(人工知能研究センター) に色々あるよ！


今回は、[カリフォルニア大学アーバイン校](https://academic-accelerator.com/Manuscript-Generator/jp/California-Irvine) Machine Learning データセットのリポジトリから『顧客解約予測データセット』（公開されている）


### | クエリ

```SQL
#standardSQL

/* MODEL_DEVLOP */

    create or replace model `my_dataset`.AUTO_ML_1 -- モデル名
    options(
           model_type = 'AUTOML_CLASSIFIER' -- 使用アルゴリズム
         -- , input_label_cols = [''] -- ターゲット名（カラム）
         , budget_hours = 1.0 -- 時間制限
     ) as
     /* クエリ（学習に使用するデータを抽出する） */
     select
         State
         , Account_Length
   　    , Area_Code
         , Total_Charge / Account_Length as avg_daily_spend
         , CustServ_Calls / Account_Length as avg_daily_cases
         , Churn_ as label -- 推論するラベル
     from  
         `my_dataset.CSV_CUSTOMER_ACTIVITY`
     where
         date(2020, 1, 1) <= Record_Date
     ;
```
```SQL
#standardSQL
/*
 * ◇ BigQuery ML
 *   先に、「MODEL_DEVLOP」を実行。実行後、コメントアウトしてView化して同一ファイルで扱うのもあり
 *   次に、「MODEL_EVALUATE」「MODEL_PREDICT」を実行。
 */

with

/* MODEL_EVALUATE */

    evaluation as (
        select 
            * 
        from 
            ml.evaluate(model `my_dataset`.AUTO_ML_1, (
            /* サブクエリ（モデル作成に使用した特徴量を抽出するクエリ） */
            select
                State
                , Account_Length
                , Area_Code
                , Total_Charge / Account_Length as avg_daily_spend
                , CustServ_Calls / Account_Length as avg_daily_cases
                , Churn_ as label
            from  
                `my_dataset.CSV_CUSTOMER_ACTIVITY`
            where
                date(2020, 1, 1) <= Record_Date
            )
        )
    )



/* MODEL_PREDICT */

    , predict as (
        select
            predicted_label
            , prob
            , State
            , Account_Length
            , avg_daily_spend
            , avg_daily_cases
        from ml.predict(model `my_dataset`.AUTO_ML_1, (
            /* サブクエリ（予測するのに必要な特徴量を抽出するクエリ） */
            select
                State
                , Account_Length
                , Area_Code
                , Total_Charge / Account_Length as avg_daily_spend
                , CustServ_Calls / Account_Length as avg_daily_cases
                -- , Churn_ as label -- ∵推論時不要
            from  
                `my_dataset.CSV_CUSTOMER_ACTIVITY`
            where
                date(2020, 1, 1) <= Record_Date
            )
        ), unnest(predicted_label_probs)
        where
            predicted_label = 'True.' -- ∵2値分類の為
    )



/* OUTPUT */

select * from evaluation;
-- select * from predict;

```

```txt
(error)Unable to identify the label column in query statement. Either specify the label column using OPTIONS(input_label_cols=['your_label_col']) or name the label column in the data as 'label'.
```
[BigQuery ML unable to identify label column in data](https://stackoverflow.com/questions/54151811/bigquery-ml-unable-to-identify-label-column-in-data) - stack overflow
[feedbackThe CREATE MODEL statement](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create) - Google Cloud

### | Taitaic
[「BigQueryML」でSQLを書いて機械学習モデルを構築&予測できる！](https://qiita.com/s_yaginuma/items/b692d3716dcb06416ce0) - Qiita
```sql
#standardSQL
begin
/* モデル作成 */
    create model `Kaggle_titanic`.model_titanic
    options (model_type = 'logistic_reg') as
    select
        Pclass
        , title
        , is_female
        , family_size
        , is_alone
        , age
        , embarked
        , fare
        , class_age
        , Survived as label
    from 
        `Kaggle_titanic.preprocessed_data` -- 前処理終わり
    left join 
        `Kaggle_titanic.label_train` using(PassengerId)
    where 
        train_flag = 'train'
    ;
/* モデル評価 */
    select * from ml.evaluate(model `Kaggle_titanic`.model_titanic, (
        select Pclass, title, is_female, family_size, is_alone, age, embarked, fare, class_age, Survived as label
        from `Kaggle_titanic.preprocessed_data`
        left join `Kaggle_titanic.label_train` using(PassengerId)
        where train_flag = 'train'
        )
    );
/* モデル推論（テストデータに対して） */
    select * from ml.predict(model `Kaggle_titanic`.model_titanic, (
        select Pclass, title, is_female, family_size, is_alone, age, embarked, fare, class_age, Survived as label
        from `Kaggle_titanic.preprocessed_data`
        where train_flag = 'test'
        )
    );



/* 係数算出（特徴量の重み） */
    select 
        processed_input
        , weight
    from 
        ml.weights(model `Kaggle_titanic.model_titanic`)
    order by  
        abs(weight) desc
    ;

/* サブミット作成 (※Kaggle用) */
    select 
        PassengerID
        , predicted_label as Survived
    from ml.predict(model `Kaggle_titanic`.model_titanic, (
        select PassengerID, Pclass, title, is_female, family_size, is_alone, age, embarked, fare, class_age, Survived as label
        from `Kaggle_titanic.preprocessed_data`
        where train_flag = 'test'
        )
    );
end
```


## || REFERENCE
+ [SQLだけでモデルが作れる！BigQuery ML による自動モデル作成と Tableau による可視化](https://www.skillupai.com/blog/tech/bigquery-tableau/) - SkillUpAI
+ [AutoML Tables が BigQuery ML で一般提供に](https://cloud.google.com/blog/ja/products/data-analytics/automl-tables-now-generally-available-bigquery-ml) - Google Cloud
+ [BigQuery上のGA4データを元にしたGoogle Auto ML Tablesの試用レビュー](https://www.principle-c.com/column/ga/ga4/review-ga4-google-auto-ml-tables/) - PRINCIPLE
+ [「BigQueryML」でSQLを書いて機械学習モデルを構築&予測できる！](https://qiita.com/s_yaginuma/items/b692d3716dcb06416ce0) - Qiita
+ [BigQuery MLでスロット使用量が急増しているプロジェクトやユーザーを異常検知する](https://www.yasuhisay.info/entry/2022/03/07/104500) - yasuhisa's blog

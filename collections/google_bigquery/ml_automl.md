## || BigQueryMLでAutoML
```sql
#standardSQL
begin
/* モデル作成 */
    create or replace model `0030997_hirukawa`.auto_ml_1 -- モデル名
    options(
          model_type = 'AUTOML_CLASSIFIER' -- 使用アルゴリズム
        -- , input_label_cols = [''] -- ターゲット名（カラム）
        , budget_hours = 1.0
    ) as
    select
        State
        , Account_Length
        , Area_Code
        , Total_Charge / Account_Length as avg_daily_spend
        , CustServ_Calls / Account_Length as avg_daily_cases
        , Churn_ as label
    from  
        `0030997_hirukawa.CSV_CUSTOMER_ACTIVITY`
    where
        date(2020, 1, 1) <= Record_Date
    ;
/* モデル評価 */
    select * from ml.evaluate(model `0030997_hirukawa`.auto_ml_1, (
        select
            State
            , Account_Length
            , Area_Code
            , Total_Charge / Account_Length as avg_daily_spend
            , CustServ_Calls / Account_Length as avg_daily_cases
            , Churn_ as label
        from  
            `0030997_hirukawa.CSV_CUSTOMER_ACTIVITY`
        where
            date(2020, 1, 1) <= Record_Date
        )
    );
/* モデル推論 */
    select
        predicted_label
        , prob
        , State
        , Account_Length
        , avg_daily_spend
        , avg_daily_cases
    from ml.predict(model `0030997_hirukawa`.auto_ml_1, (
        select
            State
            , Account_Length
            , Area_Code
            , Total_Charge / Account_Length as avg_daily_spend
            , CustServ_Calls / Account_Length as avg_daily_cases
            , Churn_ as label
        from  
            `0030997_hirukawa.CSV_CUSTOMER_ACTIVITY`
        where
            date(2020, 1, 1) <= Record_Date
        )
    ), unnest(predicted_label_probs)
    where
        label = 'True.'
    ;
end
```

```txt
(error)Unable to identify the label column in query statement. Either specify the label column using OPTIONS(input_label_cols=['your_label_col']) or name the label column in the data as 'label'.
```
[BigQuery ML unable to identify label column in data](https://stackoverflow.com/questions/54151811/bigquery-ml-unable-to-identify-label-column-in-data) - stack overflow
[feedbackThe CREATE MODEL statement](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create) - Google Cloud


## || REFERENCE
+ [SQLだけでモデルが作れる！BigQuery ML による自動モデル作成と Tableau による可視化](https://www.skillupai.com/blog/tech/bigquery-tableau/) - SkillUpAI
+ [AutoML Tables が BigQuery ML で一般提供に](https://cloud.google.com/blog/ja/products/data-analytics/automl-tables-now-generally-available-bigquery-ml) - Google Cloud
+ [BigQuery上のGA4データを元にしたGoogle Auto ML Tablesの試用レビュー](https://www.principle-c.com/column/ga/ga4/review-ga4-google-auto-ml-tables/) - PRINCIPLE
+ [「BigQueryML」でSQLを書いて機械学習モデルを構築&予測できる！](https://qiita.com/s_yaginuma/items/b692d3716dcb06416ce0) - Qiita
+ 

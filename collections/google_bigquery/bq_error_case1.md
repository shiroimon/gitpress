

## || ERROR
作成したクエリの抽出結果（300万レコード程）を一時的にテーブル化しようとしたら、以下のようなエラーが出て困った。。

    To copy a table, the destination and source datasets must be in the same region. Copy an entire dataset to move data between regions.

似たような事象の人が[相談](https://www.googlecloudcommunity.com/gc/Data-Analytics/Location-error-on-BigQuery/m-p/424261)していた。（*2）

だがしかし、分からん。。



## || REFERENCE
+ [データセットのコピーの設定](https://cloud.google.com/bigquery/docs/copying-datasets?hl=ja#setting_up_a_dataset_copy) - GoogleCloud 
+ [Location error on BigQuery](https://www.googlecloudcommunity.com/gc/Data-Analytics/Location-error-on-BigQuery/m-p/424261) - GoogleCloud(*2)

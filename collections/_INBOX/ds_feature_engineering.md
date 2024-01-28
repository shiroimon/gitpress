---
date    : 2022-01-01
title   : 特養量エンジニアリング
excerpt : 
tags    : ["Feature Engineering", "ML", "Preprocessing"]
---
## || 特養量エンジニアリング(Feature Engineering)


### | エンコーディング

- StandardSaler
- Normalization
- LabelEncoder(OrdinalEncoder)
- OneHotEncoder
- TargetEncoder

#### ▼ 向き不向き
|-|good|bad|-|
|:-|:-|:-|:-|
|StandardSaler|-|-|-|
|Normalization|-|-|-|
|LabelEncoder　　|決定木系　|-|軽いが解釈性が減る|
|OneHotEncoder|NN 線形回帰|-|重くなる|
|TargetEncoder|-|-|Overfitting(過学習)の危険|


## || REFERENCE
- [特徴量エンジニアリング](https://www.datarobot.com/jp/wiki/feature-engineering/#:~:text=%E7%89%B9%E5%BE%B4%E9%87%8F%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%8B%E3%82%A2%E3%83%AA%E3%83%B3%E3%82%B0%E3%81%A8%E3%81%AF%E3%80%81%E6%A9%9F%E6%A2%B0%E5%AD%A6%E7%BF%92%E2%80%8B%E2%80%8B%E3%83%A2%E3%83%87%E3%83%AB,%E3%81%AB%E8%BF%BD%E5%8A%A0%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%81%A7%E3%81%99%E3%80%82) - DataRobot
- [特徴量エンジニアリング](https://qiita.com/tk-tatsuro/items/f27c012e0cb95a5f51d2) - Qiita
- [【初心者】特徴量エンジニアリングについて調べてみた](https://qiita.com/zumax/items/3b6e771ef1fcaf3edd91) - Qiita

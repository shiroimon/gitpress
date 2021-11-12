---
date   : 2021-11-11
title  : 【Tableau Desktop（基礎編）】セクション03
excerpt: Tableau（タブロー）で実践！ビジネスユーザのためのデータ集計・視覚化・分析 基礎編 「カテゴリ分析（棒グラフによる可視化）」
tags   : ["Tableau Desktop", "BI", "Udemy"]
---
## || 棒グラフ作成
サブカテゴリ別の売上全体を見るため。シェルフにD&D若しくは、サイドバーのカードをダブルクリックして設定。
![](https://i.gyazo.com/a52b506fecb0c1b6e0f5d96c27215696.png)

## || ソート（並べ替え）
ツールバーの以下の絵文字のボタン押下で降順、その隣が昇順。
![](https://i.gyazo.com/4c5f43f454dd48f1ac74e4c13bc0c3a3.png)

![](https://i.gyazo.com/acc900bb00edd60b7896bf3268a7ad4b.png)

## || スワップ（軸入れ替え）
ツールバーの以下の絵文字のボタン押下で軸を入れ替えることができる。<br>
また`行シェルフ`、`列シェルフ`のカードも入れ替わっていることにも注意。
![](https://i.gyazo.com/aa9fe2c86090e4366db744d6d1914518.png)

![](https://gyazo.com/283f705d2682f3d4eb7f7623da580664.png)

## || マークの活用①（色・ラベル等）
### | ラベル
売上カードを複製（⌘を押下しながらカーソル移動）して、「マーク」内のラベルボタンに入れる。
サイドバーを右クリックして、「既定のプロパティ」>「数値設定」
![](https://i.gyazo.com/97b30ea86c0c1b6a4d9ca1c5ccae6100.png)
今回は、「百万（M）」単位で丸める。
![](https://i.gyazo.com/1d142b35bca430d5f8dad8ba681c1a37.png)

![](https://i.gyazo.com/90bf2dc4b54fd069fab140ca67ac11ba.png)

### | 色
##### ディメンジョン
サイドバーから「カテゴリ」カードをマーク内の「色」に入れる。
![](https://i.gyazo.com/b8f6a0ee49dc392f3313ed2dda42243e.png)
サイドバーから「顧客区分」カードをマーク内の「色」に入れる。
![](https://i.gyazo.com/726918adf5722fac35cbcdf3786bb6d5.png)
#### メジャー
サイドバーから「利益」カードをマーク内の「色」に入れる。
![](https://i.gyazo.com/ef2cd705ce1a8a8bae19139f48c13f13.png)

## || フィルタ（絞り込み）
#### ディメンションのフィルター
カーソルを向けて、クリックすると、詳細がモーダルする。
![](https://i.gyazo.com/c56ede201375df7cdcf59fd6c33f10f5.png)

* 保持
![](https://i.gyazo.com/33889907bf29453b251a57277edd965b.png)
* 除外
![](https://i.gyazo.com/48c7d243b63e233c991d767eedf33c65.png)

#### その他の方法
![](https://i.gyazo.com/30370d4d41e39bff349a7ccf3a3ff75e.png)

![](https://i.gyazo.com/76ae570ebc9a6b3f827f70b2424bfc5a.png)

#### 更に詳細なフィルタ
売上上位5位を表示したい。<br>
フィルター内にあるカードを右クリックして、フィルターの編集を押下。
![](https://i.gyazo.com/2af412af66e501b741d3784d59248f58.png)
上位タブに移動して、フィールド指定：にチェックを入れて、「5」を記入。
![](https://i.gyazo.com/f1afe530dbc07afac2bb0fa45e455575.png)

![](https://i.gyazo.com/8e53fdf2f1a4b65013d3480117fb37f3.png)

#### メジャーのフィルター
売上１千万円いかを除外。<br>
![](https://i.gyazo.com/82ef92555990f26cbaca93732db4dcdd.png)
![](https://i.gyazo.com/33ff742ab857a4198da222da599d81fc.png)
![](https://i.gyazo.com/6498334f7d0e9d92e2435cd75879cad0.png)

#### フィルター解除
![](https://i.gyazo.com/a3ba15f4351b75ce2d24d60390d6166d.png)


## || 階層化とドリルダウン
⚠️ サイドバーの下の階層から上階層にD&Dをすること。
![](https://i.gyazo.com/5bdb69d042754c68f0f0eecfd43f7b48.png)
![](https://i.gyazo.com/31c226e955c47c9b77c91e365298ea9e.png)
列シェルフのカード横の[+]を押下するとドリルダウンすることができる。
![](https://i.gyazo.com/8490a78e870dbaacec5d440b5c8a5fef.png)
![](https://i.gyazo.com/3fd6d347b6a0c02d78e3c21aa6c8f81e.png)

![](https://i.gyazo.com/cecc4dc988ddccd686e941f15a5ba106.png)

## || グルーピング
![](https://i.gyazo.com/397ca83eadeb8eb62bee5480f0ffd35e.png)
![](https://gyazo.com/bf60c504904a05025677baf1717f27fe.png)
![](https://gyazo.com/8b2ab3949ee74cd65ac6b26ef5dddfee.png)
![](https://gyazo.com/6dba671912090fa645646eaba5fbd870.png)
![](https://gyazo.com/3eac674d74b184b70cd7743476278edb.png)
![](https://gyazo.com/185fdf33dc4840a0b9219f501ec2df84.png)



## || リファレンスラインの基本
![]()

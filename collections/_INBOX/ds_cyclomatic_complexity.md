---
date    : 2024-01-01
title   : ｻｲｸﾛﾏﾃｨｯｸ複雑度(Cyclomatic Complexity)
excerpt : ---
tags    : ["💩", "", ""]
---

## || サイクロマティック複雑度とは

平たく言うと、いかにうんコード(💩)なのかを定量計測するよ。


### 目安
|循環的複雑度|複雑さの状態|バグ混入確率|
|:-|:-|:-|
|10以下|非常に良い構造            |25%|
|30以上|構造的なリスクあり        |40%|
|50以上|テスト不可能              |70%|
|75以上|いかなる変更も誤修正を生む|98%|



## || e.g 標準偏差関数

Pythonでスクラッチしてみて

#### サイクロマティック複雑度無視（意識せず記述）

#### サイクロマティック複雑度下げ目（意識して記述）




## || REFERENCE
- [循環的複雑度](https://ja.wikipedia.org/wiki/%E5%BE%AA%E7%92%B0%E7%9A%84%E8%A4%87%E9%9B%91%E5%BA%A6) -Wikipedia
- [循環的複雑度とは?](https://jp.mathworks.com/discovery/cyclomatic-complexity.html) -MathWorks
- [悪いコードを見つける手がかり　サイクロマティック複雑度とは？](https://emb-sw-eng.com/cyclomatic_complexity/) -ソフおも
- [サイクロマティック複雑度の下げ方・考え方～目指せ良いコード～](https://emb-sw-eng.com/cyclomatic_complexity_2/) -ソフおも
- [バグの出にくいコードを書く~サイクロマティック複雑度について~](https://www.w2solution.co.jp/corporate/tech/eg_ns_rs_cyclomaticcomplexity/) -W2


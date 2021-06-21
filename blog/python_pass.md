---
title: 【Python】 参照渡し、値渡し
date: 2021-02-21
tag: ["Python3", "参照渡し", "値渡し"]
excerpt:
---

## 結論

```
参照渡し(浅いコピー:Shallow Copy): 同じ参照元からデータを渡される
値渡し(深いコピー:Deep Copy): 参照元は別でデータを渡される
```

> `cf.`
> [copy --- 浅いコピーおよび深いコピー操作 - Python3.9,2](https://docs.python.org/ja/3/library/copy.html)


## 値渡し

【問題】：　以下の手順で作成する配列1. 2. は其々同じものでしょうか？

  1. テキトーなNumPy配列（`ndarray`）を作成。
  2. 1.の配列をコピー（`ndarrya_copy`）。

```Python
import numpy as np

ndarray = np.arange(0, 5)
ndarray_copy = mdarry.copy()

print('original arrays value is {}, id: {}.'.format(ndarray, id(ndarray)))
print('original arrays value is {}, id: {}.'.format(ndarray, id(ndarray)))
```

【結論】：　値は同じ（＝値を渡している）。でも〜idは其々違う！

  （えっ。。。idってなに？　ってなった方、次項へ）

## 参照渡し

問題：　以下のコードを実行すると`id`はどうなるでしょう？

```Python
a = 'test'
print('id:', id(a), sep='')

def f(n):
    print('id:', id(n), sep='')

f(a)
```

結論：　`id`は同じ。

  aという変数が宣言された時にはある`id`が割り振られる。（割り当てられたメモリ先に保存される。）<br>
  f(a)を実行したときに、f()の引数parametersにaの情報が渡されるわけですが、<br>
  このparametersの`id`をみても、同じIDが割り当てられている。<br>
  つまり、Pythonでは関数に引数を渡す際には、<br>
  <br>
  ❌ 値をコピーして渡しているのではなく<br>
  ⭕️ メモリの参照先(アドレス)を渡している (これを「参照渡し」といい、逆に値をコピーして渡すやり方を「値渡し」と言います.)

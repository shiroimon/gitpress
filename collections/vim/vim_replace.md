---
date   : 2024-01-01
title  : 📗 置換
excerpt: ---
tags   : ["📗", "CLI", "Vim", ""]
---


## || 内部一括置換
```
# ファイル内一括置換
:%s/{old}/{new}/g
```

```
# 行内一括置換
:s/{old}/{new}/g
```


### | 範囲指定
```
# 10~20行の内一括置換
:10,20/{old}/{new}/g
```

```
# [V] visual-mode で選択内一括置換
:'<,'>s/{old}/{new}/g
```



## || REFERENCE
- [Vimの置換コマンドの使い方](https://motw.mods.jp/Vim/substitution.html) - Memo on the web
- [vim 文字列置換 基本的な事](https://qiita.com/shirochan/items/a16487d0739f455b5e8a) - Qiita
- []() - 



---
date   : 2024-01-01
title  : git reset 
excerpt: ---
tags   : ["cli", "git", "reset"]
---

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/300px-Git-logo.svg.png)

cf. https://ja.wikipedia.org/wiki/Git

## || $git reset
### | motivation

誤って「add」「commit」してしまった！を回避。


### | $git add を取り消したい
```shell
# 全ファイル消し
$ git reset HEAAD
# 個別ファイル消し
$ git reset HEAD {ファイル名}
```

#### 初回addはこれ
```shell
# init 直後の add に対して
$ git rm --cached -r {ファイル名}
```


### | $git commit を取り消したい
```shell
$ git reset {コミットID}
```


## || reference
- [git add を取り消す](https://qiita.com/yukure/items/89562e5eb1d03995dc5b) -Qiita
- [【Git入門】git commitを取り消したい、元に戻す方法まとめ](https://tech-blog.rakus.co.jp/entry/20210528/git) -Hatenablog
- []() -


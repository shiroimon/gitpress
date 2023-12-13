--
date   : 2023-12-13
title  : GitHub
excerpt: 不要になったブランチ消込
tags   : ["", "github", "CLI"]
---

## || ブランチ

### | ブランチの消し方（ファイル指定）
```shell
$ git branch -d {your branch name}
```

### | ブランチの消し方（まとめて）
```shell
$ git branch --merged | egrep -v "(^\*|main|master|develop)" | xargs git branch -d
```
コマンド内容詳細はreference先へ！



## || REFERENCE
- [Gitブランチの一括削除! 煩雑な作業を一行で解決する方法](https://qiita.com/itinerant_programmer/items/dbf7cdba08a5403234ea) -Qiita

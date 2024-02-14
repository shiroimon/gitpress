---
date   : 2023-12-13
title  : 📍GitHub Branch
excerpt: ---
tags   : ["📍", "github", "CLI"]
---

![branch](https://github.com/sh16ma/gitpress/assets/150888300/ba0c75c6-a2da-4804-b2c5-621ea674b5b5)

## || ブランチとは


### | GitHub FLow

ブランチを知る上で、何故ブランチを切るのか？

> [コチラ]()


## || 作りたい

1. 現在のブランチ状況確認
```shell
$ git branch 
＊ main
```
2. ブランチ作成
```shell 
$ git branch {your branch name}
```
※こっちの方がよく使う。（ブランチ作成してそのブランチに入る）
```shell
$ git checkout -b {your branch name}
```



## || 消したい
### | ブランチ消し方（ファイル指定）
```shell
$ git branch -d {your branch name}
```

### | ブランチ消し方（まとめて）
```shell
$ git branch --merged | egrep -v "(^\*|main|master|develop)" | xargs git branch -d
```
コマンド内容詳細は[reference先](Gitブランチの一括削除!煩雑な作業を一行で解決する方法)へ！


cf. [Gitブランチの一括削除! 煩雑な作業を一行で解決する方法](https://qiita.com/itinerant_programmer/items/dbf7cdba08a5403234ea) -Qiita



## | リネームしたい
1. 名前を変更したいブランチに移動
```shell
$ git checkout {(old) 変更前ブランチ名}
```
2. リネーム
```shell
$ git branch -m {(new) 変更後ブランチ名}
```

cf. [「git rename」を使用してGitのブランチ名を変更する方法](https://kinsta.com/jp/knowledgebase/git-rename-branch/#git-1) -Kinsta 




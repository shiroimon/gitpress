---
date    : 2024-01-01
title   : 🐚 History
excerpt : ---
tags    : ["🐚", "shell", "cli", "zsh"]
---

![zsh](https://github.com/sh16ma/gitpress/assets/150888300/6e226d99-cc58-42c0-a307-fc221b258455)

## || $ history
### | 過去に入力したコマンドを見れる
```sh
# デフォルトで最新の16行が返される
$ history
# 0行目以降のhistoryを取得(= 全履歴表示)
$ history 0
```
### | 構成
```sh
$ history [start-row][end-row]
```
e.g `$ history 10 20` : 10行目から20行目までのhistoryを取得


## || REFERENCE
- [[zsh]historyを全て表示する方法](https://rb-station.com/blogs/software/zsh-all-history) - RoboStation


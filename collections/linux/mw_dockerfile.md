---
date    : 2023-3-23
title   : 🐳 Dockerfile
excerpt : 
tags    : ["docker_file", "CLI",]
---

![publicdomainq-0020603gmd](https://user-images.githubusercontent.com/28585421/194487142-42f7189e-b156-453c-b4e2-e39c9445f75a.jpg)
cf. https://publicdomainq.net/container-ship-0020603/

## || Dockerfile
### | instraction (基本)
1. **FROM** : `FROM {dockerimage} # OS等々指定` 
2. **RUN** : `RUN {linux comand} # やりたいこと`
3. **CMD** : 

#### 2. RUN
* ig.
```
RUN touch test
RUN echo `hello world` > test
```
(注意)　RUN を複数書きすぎてファイルが重くなる問題　→ Dockerimageのレイヤー数は最小に！

* `RUN`の他に、`COPY`、`ADD`がレイヤーを作成するインストラクション
* `&&`: コマンド結合
* `\`: 改行
* `apt`: ubuntuのパッケージ管理(パッケージインストール)
    - `RUN apt-get install {package}`
* `RUN apt-get update`: 最新版取得
* レイヤー数削減
```txt
# (Layer数:4)
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install XXX
RUN apt-get install YYY
RUN apt-get install ZZZ
```
```txt
# (Layer数:2)
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install XXX YYY ZZZ
```
```txt
# (Layer:1)
# コマンドをまとめ、改行で見やすく
FROM ubuntu:latest
RUN  apt-get update && apt-get install \
     XXX \
     YYY \
     ZZZ
```

#### 3. CMD
* コンテナのデフォルトコマンドを指定。
```
CMD ["executable", "param1", "param2"]
```
* Dockerfileの最後に記述（原則）



## || 実践
```txt
FROM ubuntu:latest
RUN  apt-get update 
RUN  apt-get install -y \
     curl \
     cvs \
     nginx 
```

### | 実践 - Dockerfile(分析基盤用)

https://github.com/polar-beer/my_docker



## || REFERRENCE
- []() - 

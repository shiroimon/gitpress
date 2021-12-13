---
date    : 2021-12-15
title   : 【🐳 Docker】
excerpt : 
tags    : ["docker", ""]
---

## || 

* バージョン確認
```shell
$ docker --version
```
* 
```shell
$ docker login
( $ docker login --username {ユーザー名})
```
Cf. [Docker Hubでパーソナルアクセストークンが利用可能になりました！](https://www.creationline.com/lab/29979) - CL LAB

### | イメージ
* 
```shell
$ docker pull {イメージ名(:laytest)}
```
(:laytest) は、オプション。最新版をDockerhubから取得できる。
* 
```shell
$ docker images
```

### | コンテナ
* コンテナ生成
```shell
$ docker run {イメージ名}
```
```shell
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
* コンテナ確認
```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
※`docker ps`コマンドはアクティブなコンテナのみ表示する。
```shell
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
d93e08b7d54b   hello-world   "/hello"   10 minutes ago   Exited (0) 10 minutes ago             vigorous_kapitsa
```
`-a`のオプションを付けると全て（all）表示される。


### | 
```shell
$ docker run -it ubuntu bash
```
> ubuntuイメージからコンテナを生成して、内部のbashのプログラミングを実行。









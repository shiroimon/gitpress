---
date    : 2021-12-15
title   : 【🐳 Docker】
excerpt : 
tags    : ["docker", ""]
---

## || Docker
*  `$ docker --version`
*  `$ brew install docker `
*  `$ docker login`
*  `$ docker logout`
*  `$ docker pull {imagename}`
*  `$ docker images`
*  `$ docker run {imagename}`
*  `$ docker ps -a`
* 全コンテナ停止: `docker stop $(docker ps -q)`
* 全コンテナ削除: `docker rm $(docker ps -q -a)`
* 全イメージ削除: `docker rmi $(docker images -q)`
* コンテナ更新: `$ docker commit {imageid/name} {new_imagename(:tag)}`
* コンテナ名変更: `$ docker tag {new_imagename(:tag)} {target}`
* コンテナをDockerhubへ: `$ docker push {imagename}`
*  `$ docker `
*  `$ docker `
*  `$ docker `
*  `$ docker `


### | How to
* バージョン確認
```shell
❯ docker --version
```
* インストール
```shell
❯ brew install docker
```

* ログイン
[Docker hub]()にて、アカウントを開設。
```shell
❯ docker login

(❯ docker login --username {ユーザー名})
username: 
🗝:
```
Cf. [Docker Hubでパーソナルアクセストークンが利用可能になりました！](https://www.creationline.com/lab/29979) - CL LAB

### | 💭 Docker Image
* Docker hubからイメージをプルする。
```shell
❯ docker pull {イメージ名(:laytest)}
```
`(:laytest)` は、オプション。デフォルトで最新版をDockerhubから取得できる。

* Docker image 一覧
```shell
❯ docker images
```

### | 📦 Docker Container
* コンテナ生成
```shell
❯ docker run {イメージ名}
```

i.g.
```shell
❯ docker run hello-world
```
```txt
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

* コンテナ一覧
```shell
❯ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
※`docker ps`コマンドはアクティブなコンテナのみ表示する。

```shell
❯ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
d93e08b7d54b   hello-world   "/hello"   10 minutes ago   Exited (0) 10 minutes ago             vigorous_kapitsa
```
`-a`のオプションを付けると全て（all）表示される。

Cf. [コマンドでDockerコンテナを停止・削除、イメージの削除をする](https://qiita.com/shisama/items/48e2eaf1dc356568b0d7) - Qiita


### | 別のコンテナを作成して起動まで 
ubuntuイメージからコンテナを生成して、内部のbashのプログラミングを実行。
```shell
❯ docker run -it ubuntu bash

root@999a5f9187cb:/# 
root@999a5f9187cb:/# pwd
root@999a5f9187cb:/# touch sample.txt
root@999a5f9187cb:/# exit

❯ docker ps -a
```

* 再起動（コンテナ）
```shell
❯ docker restart {CONTAINER ID | NAMES}
```

* 再起動（コンテナ内プログラム）
```shell
❯ docker exec -it {CONTAINER ID | NAMES} bash 
```
`-it`はshellを用いる際に必要なオプション。


## || 「Docker image」の更新方法
### | 「Docker file」→「Docker image」

### | 「container」→「Docker image」
1. 
```
❯ docker commit {imageid/name} {image_new_name(:tag)}
```
```shell
❯ docker commit 2ebd568ec16c ubuntu:updated
sha256:18033f7a50bed9e96d12e8bb4b290b82b813e48ccdfc26820d13eef30dd4fcda
❯ docker images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
ubuntu        updated   18033f7a50be   24 seconds ago   72.8MB
ubuntu        latest    ba6acccedd29   2 months ago     72.8MB
hello-world   latest    feb5d9fea6a5   2 months ago     13.3kB
```

2. 「Docker hub」で新たにレポジトリを作成
※１imageに１レポジトリの認識。

3. 新たに作成した「Docker image」を「Docker hub」のレポジトリに公開。
* リネーム
リポジトリネームに揃えて、プッシュに備える。
```
❯ docker tag {source} {target}
```
```shell
❯ docker tag ubuntu:updated d0tth/my-first-repo
❯ docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
d0tth/my-first-repo   latest    18033f7a50be   27 minutes ago   72.8MB
ubuntu                updated   18033f7a50be   27 minutes ago   72.8MB
ubuntu                latest    ba6acccedd29   2 months ago     72.8MB
hello-world           latest    feb5d9fea6a5   2 months ago     13.3kB
```
* 公開
```shell
❯ docker push d0tth/my-first-repo
```


※「Docker hub」ではない会社規定のレジストリに自作imageをあげる場合。
(有名どころは「JFrog Artifactory」...etc.)
```
{hostname}:{port}/{username}/{reposigory}:{tag}
```
```
registry-1.docker.io/libray/ubuntu"latest
```
特別指定をしないとデフォルトで「registry-1.docker.io」(Docker hub)に上がる仕組み。







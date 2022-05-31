---
date    : 2021-12-15
title   : 【🐳 Docker】基本操作＆コマンド一覧
excerpt : Dockerの基本的操作
tags    : ["docker"]
---

## || Docker

* バージョン確認: `docker --version`
* インストール  : `brew install docker `
* ログイン      : `docker login`
* ログアウト    : `docker logout`

#### 💭 image
* イメージ一覧  : `docker images`

##### Dockerhub → Dockerimage
* イメージ取得  : `docker pull {image}`

##### Dockerfile → Dockerimage
* イメージ化 : `docker build {directory}`
  + Dockerfile格納上のディレクトリ上で`docker build .`　（.はcdの意）
  + 名前指定してビルド`docker build -t {name} {directory}`

##### run

    内部的にpull&startをしている。

* イメージ実行（コンテナ起動） : `docker run -it {image} bash`
  + コンテナ起動後に削除 : `docker run -it --rm {image} bash`
  + ホストのファイルシステムをコンテナにマウント: `docker run -it -v {host}:{container} {imege}`
    - e.g. `docker run -it -v ~/host/mounted_folder:/new_dir {image} bash`
  + ユーザーID、名前を指定してコンテナ作成 : `docker run -u {UserId}:{UserGroup}`
    - e.g. `docker run -it -u $(id -u):$(id -g) -v ~/host/mounted_folder:/new_dir {image} bash`
    - PC userid  : `$ id -u`
    - PC groupid : `$ id -g`
  + ホストのポートをコンテナポートに繋げる : `docker run -p {host_port}:{container_port}`
    - e.g. `docker run -it -p 8888:8888 --rm jupyter/datascience-notebook bash`
  + コンテナがアクセスできるCPUI上限 : `docker --cpus {# of CPUs}`
  + コンテナがアクセスできるメモリ上限: `docker --memory {byte}`
    - 物理コア数 : `$ sysctl -n hw.physicalcpu_max`
    - 論理コア数 : `$ sysctl -n hw.logicalcpu_max`
    - メモリ(byte): `$ sysctl hw.memsize` 
    - e.g. `docker run -it --rm --cpus 2 --memory 2g ubuntu bash`

* イメージ削除  : `docker rmi {image}`
  + 全イメージ削除: `docker rmi $(docker images -q)`


##### file → image
* Dockerfile → Dockerimage : `docker build {directory}`
  + Dockerfileの格納されているディレクトリ上で`docker build .`　（.はcdの意）
  + 名前指定してビルド`docker build -t {name} {directory}`


#### 📦 container
* コンテナ一覧  : `docker ps -a` (ps=process status)
* コンテナのあらゆる情報確認 : `docker inspect {container}`
  + e.g.CPU数やメモリ量等確認時に : `docker inspect 5f90be76cd31 | grep -i cpu `  (| grep :抽出 -i:ignore大文字小文字問わず {検索語句} )
* コンテナ再起動: `docker restart {container} `
  + コンテナ実行  : `docker exec -it {container} bash`
* コンテナ停止  : `docker stop {container}`
  + 全コンテナ停止: `docker stop $(docker ps -q)`
* コンテナ削除  : `docker rm {container}` 
  + 全コンテナ削除: `docker rm $(docker ps -q -a)`
  + 全コンテナ削除: `docker system prune`
* コンテナ名付け : `docker run --name {name}{imagename}`
* detached mode: `docker run -d {imagename}`
  + コンテナ起動後にdetachする（バックグラウンドで動かす）
* foreground mode : `docker run --rm {imagename}`
  + コンテナをExit後に削除する（使い捨てコンテナ用）

##### container → Dockerhub
* コンテナ更新  : `docker commit {imageid/name} {new_imagename(:tag)}`
* コンテナ名変更: `docker tag {new_imagename(:tag)} {target}`
* コンテナをDockerhubへ: `docker push {imagename}`


### 実践
Dockerfileを作成後に

```shell
docker run -p 8888:8888 -v ~/docker/ds_python/:/work --name my-lab 511a925b20aa
```

* `-p` : publishしてコンテナとホストのポートを設定 
* `-v` : ホストとコンテナ内部のディレクトリを共有
  + `~/docker/ds_python/:/work` :  `{ホスト内}:{コンテナ内}` 
* `--name` : dockerimage に名前を作成


---
### | How to Docker
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
Using default tag: latest
The push refers to repository [docker.io/d0tth/my-first-repo]
68ac24b7487b: Pushed
9f54eef41275: Mounted from library/ubuntu
latest: digest: sha256:4130fc0fc824589774b32486bfa7a2d6ef0e2fc20192d58b7b7bf9f5effded43 size: 736
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


## || 更に詳しくコンテナ理解
### | run がしていること
```shell
❯ docker run {imagename}
```

実務ではそこまで用いることはないが、`run`が内部で行っていることを理解することは重要。

`docker run `は以下を内部で実行している。

```shell
❯ docker create {imagename}
❯ docker start {container}
```

* コンテナを作成するだけ
```shell
❯ docker create hello-world
40566ac4aacefa07dc410330129c3f5261c83267796b0cd66cc8b0e11c57383f
❯ docker ps -a
CONTAINER ID   IMAGE                 COMMAND    CREATED          STATUS                     PORTS     NAMES
40566ac4aace   hello-world           "/hello"   14 seconds ago   Created                              strange_bhabha
bcc883fc22d0   hello-world           "/hello"   2 minutes ago    Exited (0) 2 minutes ago             mystifying_elgamal
```

* コンテナ内部のプログラムを実行（同時に、`exit`される。）
```shell
❯ docker start 40566ac4aace
40566ac4aace
```

### | コマンドを上書き

```shell
❯ docker run ubuntu pwd
```

### | -it
```shell
❯ docker run -i -t ubuntu bash
```
`-i`:input可能
`-t`:output綺麗に表示

それぞれをまとめて、`-it`

### | コンテナ名付け 
```
❯ docker run --name sample_container ubuntu
❯ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS                     PORTS     NAMES
7e009d4a4ff7   ubuntu    "bash"    8 seconds ago   Exited (0) 7 seconds ago             sample_container
```


## || Dockerfile
### | Dockerfileパスと内容
[PATH] ~/docker/Dockerfile

```
FROM ubuntu:latest
# make samplefile.
RUN touch test
```

### | Build 
```shell
❯ cd ~/docker
❯ docker build .
[+] Building 1.5s (6/6) FINISHED
 => [internal] load build definition from Dockerfile                                              0.1s
 => => transferring dockerfile: 99B                                                               0.0s
 => [internal] load .dockerignore                                                                 0.1s
 => => transferring context: 2B                                                                   0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                  0.0s
 => [1/2] FROM docker.io/library/ubuntu:latest                                                    0.0s
 => [2/2] RUN touch test                                                                          1.2s
 => exporting to image                                                                            0.0s
 => => exporting layers                                                                           0.0s
 => => writing image sha256:6720ca80961dae14f5bcc79fb0a5127cd1ee354e1c112ce23bf96492469d402b      0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
❯ docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
<none>                <none>    6720ca80961d   30 seconds ago   72.8MB
```

無名で作成するとDockerimageは[Donglingimage]()として作成される。
試しに、フィルターで確認するとDonglingimageを返す。
```shell
❯ docker images -f dangling=true
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
<none>       <none>    6720ca80961d   4 minutes ago   72.8MB
```

Cf.[使用していない Docker オブジェクトの削除（prune）](https://docs.docker.jp/config/pruning.html) - Docker-docs-ja

```shell
❯ docker build -t new-ubuntu:latest .
```

### | instraction (Dockerfileのコマンド)
#### 基本インストラクション
1. `FROM`
2. `RUN`
3. `CMD`

#### FROM
```
FROM {dockerimage} # OS等々指定する。
```
#### RUN
```
RUN {linux comand} # やりたいこと
```
ig.
```
RUN touch test
RUN echo `hello world` > test
```
* ※ RUN を複数書きすぎてファイルが重くなる問題。
   * → Dockerimageのレイヤー数は最小に。

* `RUN`の他に、`COPY`、`ADD`がレイヤーを作成するインストラクション。
* コマンドは`&&`で繋げる。
* 改行は`\`。
* パッケージインストール（`apt`はubuntuのパッケージ管理）
```
RUN apt-get install {package}
```
* 最新版取得
```
RUN apt-get update
```
* レイヤー数削減
```
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install XXX
RUN apt-get install YYY
RUN apt-get install ZZZ
```
(Layer:4)<br>
 ↓ `install`をまとめる。
```
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install XXX YYY ZZZ
```
(Layer:2)<br>
 ↓ コマンドをまとめ、改行で見やすく。
```
FROM ubuntu:latest
RUN apt-get update && apt-get install \
XXX \
YYY \
ZZZ
```
(Layer:1)

実践
```
FROM ubuntu:latest
RUN apt-get update 
RUN apt-get install -y \
    curl \
    cvs \
    nginx 
```

#### CMD
* コンテナのデフォルトコマンドを指定。
```
CMD ["executable", "param1", "param2"]
```
* Dockerfileの最後に記述（原則）



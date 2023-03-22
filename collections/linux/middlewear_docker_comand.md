---
date    : 2023-3-23
title   : 🐳 Dockerコマンド一覧
excerpt : 
tags    : ["docker", "comand",]
---

![publicdomainq-0020603gmd](https://user-images.githubusercontent.com/28585421/194487142-42f7189e-b156-453c-b4e2-e39c9445f75a.jpg)
https://publicdomainq.net/container-ship-0020603/


## || Docker コマンド一覧
* `brew install docker `: インストール
* `docker --version`: バージョン確認
* `docker login`: ログイン
* `docker logout`: ログアウト

### | FILE
* `docker build {directory}`: Dockerfile → Dockerimage
  * `docker build .`: Dockerfileの格納されているディレクトリ上で実行（.はcdの意）
  * `docker build -t {name} {directory}`名前指定してビルド

### | IMAGE
* `docker images`: イメージ一覧
* `docker pull {image}`: イメージ取得(Dockerhub → Dockerimage)
* `docker build {directory}`: イメージ化 (Dockerfile → Dockerimage)
    * `docker build .` Dockerfile格納上のディレクトリ上で実行（.=cdの意）
    * `docker build -t {name} {directory}`: 名前指定してビルド
* `docker run -it {image} bash`: コンテナ起動(イメージ実行)　※内部的に「pull」「start」をW実行している。
* `docker run -it --rm {image} bash`: コンテナ起動後に削除 
* `docker run -it -v {host}:{container} {imege}`: ホストのファイルシステムをコンテナにマウント 
    - e.g.) `docker run -it -v ~/host/mounted_folder:/new_dir {image} bash`
* `docker run -u {UserId}:{UserGroup}`: ユーザーID、名前を指定してコンテナ作成
    - e.g.) `docker run -it -u $(id -u):$(id -g) -v ~/host/mounted_folder:/new_dir {image} bash`
    - PC userid  : `$ id -u`
    - PC groupid : `$ id -g`
* `docker run -p {host_port}:{container_port}`: ホストのポートをコンテナポートに繋げる
    - e.g. `docker run -it -p 8888:8888 --rm jupyter/datascience-notebook bash`
* `docker --cpus {# of CPUs}`: コンテナがアクセスできるCPUI上限
* `docker --memory {byte}`
    - `$ sysctl -n hw.physicalcpu_max`: 物理コア数
    - `$ sysctl -n hw.logicalcpu_max`: 論理コア数
    - `$ sysctl hw.memsize` : メモリ(byte)
    - e.g.)  `docker run -it --rm --cpus 2 --memory 2g ubuntu bash`
* `docker rmi {image}`: イメージ削除
* `docker rmi $(docker images -q)`: 全イメージ削除

### | CONTAINER
* `docker ps -a`: コンテナ一覧 (ps=process status)
* `docker inspect {container}`: コンテナのあらゆる情報確認
  + e.g.) `docker inspect 5f90be76cd31 | grep -i cpu `: CPU数やメモリ量等確認時 ([|grep]=抽出 ,[-i]=ignore大文字小文字問わず {検索語句} )
* `docker restart {container} `: コンテナ再起動
  + `docker exec -it {container} bash`: コンテナ実行
* `docker stop {container}`: コンテナ停止
* `docker stop $(docker ps -q)`: 全コンテナ停止
* `docker rm {container}` : コンテナ削除
  + `docker system prune`、`docker rm $(docker ps -q -a)`: 全コンテナ削除
* `docker run --name {name}{imagename}`コンテナ名付け
* `docker run -d {imagename}`: detached mode コンテナ起動後にdetachする（バックグラウンドで動かす）
* `docker run --rm {imagename}`: foreground mode コンテナをExit後に削除する（使い捨てコンテナ用）
* `docker commit {imageid/name} {new_imagename(:tag)}`: コンテナ更新
* `docker tag {new_imagename(:tag)} {target}`: コンテナ名変更
* `docker push {imagename}`: コンテナをDockerhubへ



### | 実践
Dockerfileを作成後に

```shell
$ docker run -p 8888:8888 -v ~/docker/ds_python/:/work --name my-lab 511a925b20aa
```
* `-p`: publishしてコンテナとホストのポートを設定 
* `-v`: ホストとコンテナ内部のディレクトリを共有
    + `~/docker/ds_python/:/work` :  `{ホスト内}:{コンテナ内}` 
* `--name`: dockerimage に名前を作成



## || REFERRENCE
* [Docker Hubでパーソナルアクセストークンが利用可能になりました！](https://www.creationline.com/lab/29979) - CL LAB
* [コマンドでDockerコンテナを停止・削除、イメージの削除をする](https://qiita.com/shisama/items/48e2eaf1dc356568b0d7) - Qiita
* [使用していない Docker オブジェクトの削除（prune）](https://docs.docker.jp/config/pruning.html) - Docker-docs-ja
+ [コンテナとは何か解説、従来の仮想化と何が違う？DockerやKubernetesとは？](https://www.sbbit.jp/article/cont1/57184) - ビジネス＋IT
+ [Virtualisation and Container (仮想化とコンテナ) – Ansible, Docker and Kubernetes](https://avinton.com/academy/%E4%BB%AE%E6%83%B3%E5%8C%96%E3%81%A8%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A/) - Avinton


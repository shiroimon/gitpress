## || Docker File 

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

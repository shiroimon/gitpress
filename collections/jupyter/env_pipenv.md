---
date    : 2022-01-01
title   : 🪐 
excerpt : 
tags    : ["Jupyter", "Jupyter lab", ""]
---

## || 
* 仮想環境構築
```shell
$ pip install pipenv
```
* 任意のディレクトリに任意のフォルダを作成して移動
```shell
$ cd Dev
$ mkdir myproject
$ cd myproject
```
* Python３系指定（任意のバージョン指定も可能◎）
```shell
$ pipenv --python 3
($ pipenv --python 3.6)
```
* JupyterLabをインストール
```shell
$ pipenv install jupyterlab
```
* 仮想環境起動
```shell
$ pipenv shell
(myproject)＄ 
```
＊ JupyterLab起動
```shell
(myproject)＄ jupyter lab
```
※ ブラウザーに立ち上がる。
※ 終了する際は、ブラウザー側のWebUIで終了、もしくは `Ctrl+D` で終了。
* 仮想環境終了
```shell
(myproject)＄ exit
```

## || REFERENCE
- [図解！Jupyter Labを徹底解説！(インストール・使い方・拡張機能)](https://ai-inter1.com/jupyter-lab/) - AI-interのPython3入門
- [PythonのPipenvで仮想環境を構築する](https://medium.com/p/2fbcd681f534#264c) - medium

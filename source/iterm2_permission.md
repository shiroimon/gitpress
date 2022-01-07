---
data    : 2021-12-24
title   : 【iTearm】パーミッション
excerpt : 権限について
tags    : ["permission", "iTearm"]
---

## 権限について 
```sh
$ ls -la

drwxr-xr-x   6 root  admin     193 10 18 12:30 ..
-rw-r--r--   1 tt    staff   43295  6  2  2021 memo.ipynb
drwxr-xr-x   3 tt    staff      96 12 22 15:46 docker
drwxr-xr-x   7 tt    staff     224  1 23  2021 kaggle
```

```txt
[其々の意味]
d    ***  [-:file][d:directory][l:simbolicklink]
rwx  ***  [-:nothing][r:read][w:wright][x:execute] 所有者
rwx  ***  [-:nothing][r:read][w:wright][x:execute] 所有グループ
rwx  ***  [-:nothing][r:read][w:wright][x:execute] その他
```

### UserID/UserGroupの確認
* UserID 確認 `$ id -u`
* UserGroup 確認 `$ id -g`


## パーミッション権限変更(change mode)
```sh
$ chmod 400 {file}
```
400 = 自分は読むだけ、その他ユーザーはアクセスできない

###  数字の意味
```txt
4 : read
2 : write
1 : execute
0 : no permission
```

### 数字の効力先
`$ chmod [所有者][グループ][その他] {file}`

eg. 
* 全権限付与`4 + 2 + 1 = 7`
* 読み書き付与`4 + 2 = 6`




# dcloud-NSO-container
NSO コンテナハンズオンのステップをまとめます

## イメージの読み込みと起動
### 商用コンテナイメージの展開
work フォルダを作成し、その中で実行
```
bash ../nso-6.5-freetrial.container-image-prod.linux.x86_64.signed.bin
```


### 商用コンテナイメージの読み込み
```
docker load -i nso-6.5.container-image-prod.linux.x86_64.tar.gz
```

### 商用コンテナイメージの確認
```
docker image ls
```

### コンテナの起動
```
docker run -itd --name nso -e ADMIN_PASSWORD=admin cisco-nso-prod:6.5
```

### コンテナ NSO CLI へのアクセス（終了は exit）
```
docker exec -it nso ncs_cli -C -u admin
```

### コンテナへの bash アクセス（終了は exit）
```
docker exec -it nso bash
```

### コンテナの停止と削除
```
docker stop nso
```

```
docker rm nso
```


## フォルダのマウント
### フォルダをマウントしてコンテナイメージを起動
```
docker run -itd --name nso -e ADMIN_PASSWORD=admin --mount type=bind,source=/home/nso/nso-root,target=/nso --mount type=bind,source=/home/nso/nso-log,target=/log cisco-nso-prod:6.5
```

### ncs.conf の永続化
```
docker exec -it nso bash
```

```
cp /etc/ncs/ncs.conf /nso/etc
```


## NED のインストールとデバイス登録
### NED の展開
work ディレクトリ内に移動後下記を実行

```
bash ../ncs-6.5-cisco-iosxr-7.69-freetrial.signed.bin
```

### NED ファイルをコピー
```
docker cp ncs-6.5-cisco-iosxr-7.69.tar.gz nso:/nso/run/packages
```

### NED の読み込み

```
docker exec -it nso ncs_cli -C -u admin
```

```
packages reload
```

### デバイス登録
NSO で config terminal 実行後、load merge terminal と入力し下記をコピペ

```
devices authgroups group XR umap admin remote-name admin remote-password cisco123 remote-secondary-password cisco123
top
devices device xr-1 address 172.30.0.2 device-type cli protocol ssh ned-id cisco-iosxr-cli-7.69
authgroup XR
state admin-state unlocked
top
devices device xr-2 address 172.30.0.3 device-type cli protocol ssh ned-id cisco-iosxr-cli-7.69
authgroup XR
state admin-state unlocked

```

その後 CTRL-D を入力し commit を実行。下記実行例。

```
admin@ncs# config terminal
Entering configuration mode terminal
admin@ncs(config)# load merge terminal
Loading.
（上記コピペしして CTRL-D を入力）

0 bytes parsed in 14.75 sec (0 bytes/sec)
admin@ncs(config)# commit
Commit complete.
admin@ncs(config)# end
admin@ncs#
```





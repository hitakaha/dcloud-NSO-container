# dcloud-NSO-container
NSO コンテナハンズオンのステップをまとめます

## イメージの読み込みと起動
### 商用コンテナイメージの読み込み
docker load -i nso-6.5.container-image-prod.linux.x86_64.tar.gz

### 商用コンテナイメージの確認
docker image ls

### コンテナの起動
docker run -itd --name nso -e ADMIN_PASSWORD=admin cisco-nso-prod:6.5

### コンテナイメージへのアクセス（終了は exit）
docker exec -it nso ncs_cli -C -u admin


## フォルダのマウント
### フォルダをマウントしてコンテナイメージを起動
docker run -itd --name nso -e ADMIN_PASSWORD=admin --mount type=bind,source=/home/nso/nso-root,target=/nso --mount type=bind,source=/home/nso/nso-log,target=/log cisco-nso-prod:6.5

## NED のインストールとデバイス登録
### NED ファイルをコピー
docker cp ncs-6.5-cisco-iosxr-7.69.tar.gz nso:/nso/run/packages

### デバイス登録
```
admin@ncs# config t
Entering configuration mode terminal
admin@ncs(config)# devices authgroups group XR umap admin remote-name admin remote-password cisco123 remote-secondary-password cisco123
admin@ncs(config-umap-admin)# top
admin@ncs(config)# devices device xr-1 address 172.30.0.2 device-type cli protocol ssh ned-id cisco-iosxr-cli-7.69
admin@ncs(config-device-xr-1)# authgroup XR
admin@ncs(config-device-xr-1)# state admin-state unlocked
admin@ncs(config-device-xr-1)# top
admin@ncs(config)# devices device xr-2 address 172.30.0.3 device-type cli protocol ssh ned-id cisco-iosxr-cli-7.69
admin@ncs(config-device-xr-2)# authgroup XR
admin@ncs(config-device-xr-2)# state admin-state unlocked
admin@ncs(config-device-xr-2)# commit
Commit complete.
admin@ncs(config-device-xr-2)# end
admin@ncs#
```



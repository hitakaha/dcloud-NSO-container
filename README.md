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

## NED のインストール
### NED ファイルをコピー
docker cp ncs-6.5-cisco-iosxr-7.69.tar.gz nso:/nso/run/packages




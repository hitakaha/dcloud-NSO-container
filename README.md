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






# Login & setup
ログイン
```bash
## ローカライズしたい場合は
cf config --locale ja-JP

cf login -a api.run.pivotal.io -u shashitani@pivotal.io -o hashi-org -s development
```

# CF 基本コマンド
```bash
cf help
cf plugins
cf orgs
```

# saitamaの設定
```bash
## Org:saitama User:qualica01
# qualica01でログイン
cf login -a api.run.haas-234.pez.pivotal.io --skip-ssl-validation

# Space "Tokorozawa" を作成
cf create-space tokorozawa
# User 03/04にtokorozawaへの権限を付与
cf set-space-role qualica03 saitama tokorozawa SpaceManager
cf set-space-role qualica04 saitama tokorozawa SpaceDeveloper
```
# chibaの設定
```bash
## Org:saitama User:qualica02
# qualica02でログイン
cf login -a api.run.haas-234.pez.pivotal.io --skip-ssl-validation

# Space "Tokorozawa" を作成
cf create-space kashiwa
# User 03/04にtokorozawaへの権限を付与
cf set-space-role qualica05 chiba kashiwa SpaceManager
cf set-space-role qualica05 chiba kashiwa SpaceDeveloper
```
# 開発
## Spring Music
https://github.com/cloudfoundry-samples/spring-music
## ビルド
```bash
## cloing the repo
git clone https://github.com/cloudfoundry-samples/spring-music.git
cd spring-music
## build
./gradlew clean assemble
cf target
## manifest.ymlを編集
## cf push
cf push
```
## DBインスタンスの調達
cf marketplace
cf marketplace -s p.mysql
cf create-service p.mysql db-small hashi-spring-db
watch -n 3 cf service hashi-spring-db

## スケール
## アプリケーションとDBを接続
```
cf services
cf bind-service spring-music hashi-spring-db
cf restage spring-music
```

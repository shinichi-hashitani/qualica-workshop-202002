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
```bash
cf marketplace
cf marketplace -s p.mysql
cf create-service p.mysql db-small hashi-spring-db
watch -n 3 cf service hashi-spring-db
```

## スケール
```bash
cf scale spring-music-hashi -i 2
```

## アプリケーションとDBを接続
```bash
cf services
cf bind-service spring-music hashi-spring-db
cf restage spring-music
```

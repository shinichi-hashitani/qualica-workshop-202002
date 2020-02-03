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
cf logs spring-music
```

## DBインスタンスの調達
MySQL for Pivotal Platform

https://docs.pivotal.io/p-mysql/2-7/about.html
```bash
cf marketplace
cf marketplace -s p.mysql
cf create-service p.mysql db-small hashi-spring-db
watch -n 3 cf service hashi-spring-db
```

## スケール
ディスク、メモリ、インスタンス数（AI数）を指定してコマンドからスケール。インスタンス数を指定した場合は水平スケールとなるがその他の場合は垂直スケールとなる。
```bash
cf scale spring-music-hashi -i 2
```

## アプリケーションとDBを接続
```bash
cf services
cf bind-service spring-music hashi-spring-db
cf restage spring-music
```

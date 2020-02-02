
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
# CF Space
```
cf spaces
cf delete-space development
cf create-space dev
cf create-space prod
cf target
cf target -s dev
cf target
cf target -s prod

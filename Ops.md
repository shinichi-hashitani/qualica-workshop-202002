# Org / Space / User / Quota
## saitamaの設定
```bash
## Org:saitama User:qualica01
# qualica01でログイン
cf login -a api.run.haas-234.pez.pivotal.io --skip-ssl-validation

# Space "tokorozawa" を作成
cf create-space tokorozawa
# User 03/04にtokorozawaへの権限を付与
cf set-space-role qualica03 saitama tokorozawa SpaceManager
cf set-space-role qualica04 saitama tokorozawa SpaceDeveloper
```
## chibaの設定
```bash
## Org:saitama User:qualica02
# qualica02でログイン
cf login -a api.run.haas-234.pez.pivotal.io --skip-ssl-validation

# Space "kashiwa" を作成
cf create-space kashiwa
# User 05にkashiwaへの権限を付与
cf set-space-role qualica05 chiba kashiwa SpaceDeveloper

# saitamaのクォータを変更
cf target
cf create-quota large -m 20G -r 2000 -s -1
cf quotas
cf set-quota saitama large
```

# Ops Managerにログイン
```bash
# Jumpbox経由でOps Managerにログイン
ssh -l ubuntu ubuntu-234.haas-234.pez.pivotal.io
ssh opsmgr-01.haas-234.pez.pivotal.io

# Bosh Directorの情報を確認
om \
  --username admin \
  --password xxxxxxxx \
  --target opsmgr-01.haas-234.pez.pivotal.io \
  --skip-ssl-validation \
  bosh-env
```

# BOSH Director
BOSH DirectorにはOps Managerからアクセスする。アクセス事にBOSH DirectorのURLや証明書を環境変数に設定する必要がある。この設定方法はOps Manager（UI）から取得出来る。

Ops Managerにログインした状態で Bosh Commandline Credentials の内容をそのままexportに設定する。これでOps Managerからboshコマンドを使用しBOSH Directorを扱う事が出来る。
```bash
# Jumpboxにssh後、Ops Managerにssh。
ssh -l ubuntu ubuntu-234.haas-234.pez.pivotal.io
ssh opsmgr-01.haas-234.pez.pivotal.io
# BOSHが管理するDeplyomentの一覧
bosh deployments
# BOSH管理のVMインスタンスの一覧
bosh instances
# --vitals -d でDeploymentを指定
bosh instances --vitals -d pivotal-container-service-47401a601da02a2b127c
# ここから各プロセスのサマリも確認可能
bosh instances --vitals -d pivotal-container-service-47401a601da02a2b127c --ps
bosh instances --vitals -d service-instance_27f1d58c-04e7-4f32-8311-78ad68f8d6b5 --ps
# 単純に以下で全deploymentの全プロセスを確認可能
bosh instances --ps
```

# プロダクトのインストールと更新
プロダクトのインストールと更新はOps Manager UI上でも実施可能だが、通常CLIで実施もしくはConcourseを利用して自動化される。

今回は別環境（PKS on GCP）にてCLIを利用しプロダクトをアップデートする。

```bash
pivnet login --api-token=xxxx
# pivnetコマンドでPKSの最新パッケージを検索
pivnet products | grep container
pivnet releases -p pivotal-container-service
# 製品IDの確認
pivnet pfs -p pivotal-container-service -r 1.6.1
# 製品のダウンロード
pivnet dlpf -p pivotal-container-service -r 1.6.1 -i 582811

# omコマンドを使用してパッケージをOps Managerにアップロード
om \
  --username admin \
  --password xxxxxxxx \
  --target opsmgr-01.haas-234.pez.pivotal.io \
  --skip-ssl-validation \
  upload-product \
    --product pivotal-container-service-1.6.1-build.6.pivotal

om \
  --username admin \
  --password xxxxxxxx \
  --target opsmgr-01.haas-234.pez.pivotal.io \
  --skip-ssl-validation \
  staged-products
    
# Stemcellも製品同様にpivnetからダウンロードする
pivnet releases -p stemcells-ubuntu-xenial
pivnet pfs -p stemcells-ubuntu-xenial -r 621.50
pivnet dlpf -p stemcells-ubuntu-xenial -r 621.50 -i 589724
# omのサブコマンドがupload-productではなくupload-stemcellである点のみ異なる。
om \
  --username admin \
  --password xxxxxxxx \
  --target opsmgr-01.haas-234.pez.pivotal.io \
  --skip-ssl-validation \
  upload-stemcell \
    --stemcell bosh-stemcell-621.50-vsphere-esxi-ubuntu-xenial-go_agent.tgz
```
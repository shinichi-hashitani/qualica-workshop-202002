# Ops Managerにログイン
```bash
# Jumpbox経由でOps Managerにログイン
ssh -l ubuntu ubuntu-234.haas-234.pez.pivotal.io
ssh opsmgr-01.haas-234.pez.pivotal.io

# Bosh Directorの情報を確認
om \
  --username admin \
  --password gwRI6niPX30n1HvJEC! \
  --target opsmgr-01.haas-234.pez.pivotal.io \
  --skip-ssl-validation \
  bosh-env
```

# BOSH Directorにアクセス
BOSH DirectorにはOps Managerからアクセスする。アクセス事にBOSH DirectorのURLや証明書を環境変数に設定する必要がある。この設定方法はOps Manager（UI）から取得出来る。

Ops Managerにログインした状態で Bosh Commandline Credentials の内容をそのままexportに設定する。これでOps Managerからboshコマンドを使用しBOSH Directorを扱う事が出来る。
```bash
bosh deployments
```


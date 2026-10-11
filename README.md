# srvprotianti 公共部署资源

本仓库通过 Git LFS 保存服务端四语言完整查询卡库。服务端仓库是公开 fork，GitHub 的 LFS
接口拒绝向它新增这四份文件，因此资源在这个独立仓库发布。安装、云端测试和校验工具仍归
[srvprotianti 的 ops 目录](https://github.com/301zrg/srvprotianti/tree/restructure2/ops)。

## 当前资源

`catalog/` 中有 `cards.zh.cdb`、`cards.en.cdb`、`cards.ja.cdb`、`cards.ko.cdb`。
中文卡库提供权威卡片类型和异画 alias；其他语言补充名称。它们与引擎的两套在线 CDB、
Lua/scripts/禁限表以及网页裁剪卡池是独立部署输入，不替代实际对战引擎资源。

2026-10-11 首批资源与服务端 `ops/versions.json` 记录的 SHA256 和大小完全一致，
这里只发布既有卡库，不改变卡池、对局规则或使用率统计算法。

## 自动取得与验证

使用包含 `catalog-fetch` 的服务端版本，在服务端根目录执行：

下载工具及更新后的指南见 [服务端 PR #5](https://github.com/301zrg/srvprotianti/pull/5)。
合并前使用 `codex/catalog-lfs-20261011` 分支；合并后可使用包含该改动的 `restructure2`。

```bash
sudo apt-get update
sudo apt-get install -y git git-lfs python3
python3 ops/scripts/ops.py catalog-fetch \
  --destination plugins/card-catalog/databases --versions ops/versions.json
python3 ops/scripts/ops.py catalog-check \
  --catalog plugins/card-catalog/databases --versions ops/versions.json
```

工具按服务端清单中的完整 commit SHA 下载、验证四库，再写入新的目标目录。目标已存在时
拒绝覆盖：已有资源先执行 `catalog-check`；需要更换时下载到新的目录并按服务端手册切换。
替换后需要重启游戏服务，缺库期间漏掉的统计不会随重启自动回填。

## 手工拉取备用方式

先安装 `git-lfs`。使用服务端 `ops/versions.json` 的 `catalogSource.commit` 固定版本，
不要直接把浮动 `main` 当作生产部署版本。

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/301zrg/srvprotianti-resources.git
cd srvprotianti-resources
git lfs install --local
git checkout --detach REPLACE_WITH_CATALOG_SOURCE_FULL_COMMIT_SHA
git lfs pull --include='catalog/*.cdb' --exclude=''
git lfs fsck
```

GitHub 页面、Raw 链接或源码 ZIP 可能提供的是 LFS 指针；必须执行 LFS 拉取和服务端校验，
不能把几行指针文本当作 SQLite 卡库。更新资源时提交新的 LFS 对象、完整 commit 与 SHA256
并同步服务端清单，保留旧版本用于回退。

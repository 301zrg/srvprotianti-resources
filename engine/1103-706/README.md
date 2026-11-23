# 1103 / 706 在线引擎卡池

`cards.cdb` 是用户于 2026-10-12 授权纳入 Git LFS 的当前正式服引擎卡库原件。
只读取得后保持字节不变；发布该文件不替换正式服务器的卡库或改变规则。

| 项目 | 值 |
| --- | --- |
| 字节数 | 1,695,744 |
| `datas` / `texts` 行数 | 5,267 / 5,267 |
| SHA-256 | `a49f0652ad54a7f55c569284ca154f0ec435fc26ebdd728927052e57b9d816e5` |
| 新安装的运行文件名 | `ygopro/cards.cdb` |

它与 `catalog/` 的四语完整查询库用途不同；不能用四语库或本机另一份同名大库替代。
历史 `cards1.cdb` 不在当前正式服加载路径，本次没有发布或新增切库逻辑。

使用服务端 `ops/versions.json` 的 `enginePoolSource.commit` 固定资源提交，
执行 LFS 拉取及 SQLite/字节/SHA 校验后才安装。不要把网页原始库、网页裁剪产物或 LFS 指针作为引擎卡池。
引擎程序、基础 Lua、旧裁定覆盖和禁限表按服务端安装指南分别准备，不能仅靠这份 CDB 开服。

## 旧裁定组合件与禁限表（2026-11-23 发布）

正式服 2026-11-12 起生效的规则环境 `706-20261112`，除上面的卡池外还用到下面两个文件。用户于 2026-11-22 同意公开，原字节发布（`.gitattributes` 标为 `-text`，不做换行转换）。

| 文件 | 字节数 | SHA-256 | 运行时位置 |
| --- | ---: | --- | --- |
| `utility.lua` | 78,920 | `7b9953eac5bf1027127e9f5a058dbb5ebd1475b4cb904ca85e5014734d807cdf` | `ygopro/expansions/script/utility.lua` |
| `lflist.conf` | 666,033 | `c75a96b1bbe2ceb36db9a48e6b32ec01915bc8cfc36a9df812f4b0efac994338` | `ygopro/lflist.conf` |

- `utility.lua` 是组合件：在正式服旧 `utility.lua` 正文里 `Auxiliary.PreloadUds` 的定义之后，调用一次 `PreloadUds()`。它和 [specials](https://github.com/301zrg/specials) 标签 `706-20261112` 的 `706/` 目录（399 个文件）一起放进 `expansions/script/`，构成 400 个文件的旧裁定覆盖层。它不能放进 specials 的 `706/` 目录，否则天梯助手的更新器会报错。
- `lflist.conf` 是正式服使用的完整禁限表：第一段 `!2011.3.1` 就是 1103 禁表（134 张），其后是上游附带的各期禁表。服务端主配置 `hostinfo.lflist=0` 选的是第一段。
- 服务端 `ops/versions.json` 的 `resourceFingerprints` 锁定这两个 SHA；按服务端 [从零复现说明](https://github.com/301zrg/srvprotianti/blob/main/ops/docs/reproduce-scope.md) 取用。

## Combined utility and banlist (published 2026-11-23)

The production rule environment `706-20261112` also uses the two files above, published byte for byte with the owner's consent (marked `-text`, no line-ending conversion). `utility.lua` belongs in `expansions/script/` next to the 399 files of the specials tag `706-20261112`; it must not go into specials' `706/` directory. `lflist.conf` is the full production banlist; its first section `!2011.3.1` is the 1103 list selected by `hostinfo.lflist=0`.

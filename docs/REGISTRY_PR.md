# 提交到 Lumina-Workshop-Registry 的操作手册

本文件描述把 `studio.heightforge.heightmap` 上架到创意工坊的流程与当前状态。

## 0. 现状

| 项目 | 值 |
| --- | --- |
| 模块 ID | `studio.heightforge.heightmap` |
| 当前版本 | `1.3.3` |
| 仓库 | `https://github.com/xueseyinlang-cmyk/LUMINA-HFMAP` |
| Release | `v1.3.3`（非 draft、非 prerelease） |
| 包文件 | `studio.heightforge.heightmap-1.3.3.lumina-workshop` |
| 包大小 | 219,755 字节 |
| 包 sha256 | `24f0fde9ad26d41630878e5a0ff959304db59e4a9e7a90d241cb205006830459` |
| manifest sha256 | `a64d2740fa9b637a3e3aed780a1199409b7cccd5263fa804b8ecb75d93080ada` |
| 权限 | `image.pick`、`handoff.image` |
| 兼容 | Workshop API `1.0.0 ≤ x < 2.0.0`；Lumina `≥ 2.0.0` |
| 记录 | 已提交：Registry PR #12（追加 1.3.2 与 1.3.3），等待审核合并 |

记录文件：`registry/modules/studio.heightforge.heightmap.json`。其中 `repository` 仍是
仓库改名前的 `https://github.com/xueseyinlang-cmyk/LUMINA-`：旧 URL 302 跳转、数字仓库 ID
不变，按 Registry 既有做法不改（改动属于身份变更，需要单独审核）。

## 1. 发布 Release（1.3.3 已完成，保留步骤供后续版本参考）

1. 版本号统一到 `manifest.json`、`package.json`、`ui/index.html` 的 `MODULE_VERSION`。
2. 打包并记录哈希：

   ```bash
   python tools/build_package.py
   # 产出 dist/studio.heightforge.heightmap-<version>.lumina-workshop
   ```

3. 打 tag 并推送（本机需代理与 schannel 绕过）：

   ```bash
   git tag -a v<version> -m "studio.heightforge.heightmap <version>"
   git -c http.schannelCheckRevoke=false -c http.proxy=http://127.0.0.1:7890 push origin v<version>
   ```

4. Release 必须**非 draft、非 prerelease**，且**只挂一个**模块资产；确认这个地址能下载（Registry 会抓两次）：

   ```text
   https://github.com/xueseyinlang-cmyk/LUMINA-HFMAP/releases/download/v<version>/studio.heightforge.heightmap-<version>.lumina-workshop
   ```

> 资产字节必须与本地 `dist/` 一致；重新打包后摘要会变，记录要跟着重新生成。

## 2. 生成记录

```bash
python tools/make_registry_record.py   --repository https://github.com/xueseyinlang-cmyk/LUMINA-HFMAP   --publisher "HeightForge Studio"   --package dist/studio.heightforge.heightmap-<version>.lumina-workshop
```

脚本会核对 tag 与版本、资产名、字节数、两个 sha256、权限、兼容范围、SemVer 递增；
任何一项对不上都会 `[fail]` 而不是写出文件。

追加新版本时保留既有 `versions`：记录只追加，历史条目不可修改。

## 3. 提 PR

在 `Lumina-Workshop-Registry` 仓库更新 `modules/studio.heightforge.heightmap.json`：

- 只在 `versions` 末尾追加新条目；
- PR 正文写明版本、资产名、字节数、`sha256` / `manifestSha256`、权限是否变化、兼容范围。

当前 PR：#12（1.3.2 + 1.3.3），分支 `add/studio.heightforge.heightmap-1.3.2`。

## 4. CI 与审核会检查什么（对应本记录的字段）

| Registry 检查 | 本记录对应处 |
| --- | --- |
| 仓库是精确 HTTPS GitHub 地址 | `repository` |
| tag = `v<version>` 且非 draft/prerelease | `releaseTag` |
| 资产名 = `<moduleId>-<version>.lumina-workshop` | `assetName` |
| 字节数一致 | `assetBytes` |
| 包 sha256 一致 | `sha256` |
| 包内 manifest 的 sha256 一致 | `manifestSha256` |
| 包内 manifest 的 id/version 与记录一致 | `moduleId` / `version` |
| 权限与审核记录一致、且只在白名单内 | `permissions` |
| 兼容范围必须大于等于实测 | `workshopApi` / `luminaVersion` |
| 历史版本只追加、严格递增 | `versions[]` 顺序 |
| 两次下载字节稳定 | 由 CI 抓两次比较，对应本仓库的确定性构建 |

## 5. 可选：本地安装验证

步骤见 `docs/LOCAL_INSTALL.md`（已按 1.3.3 更新）。桌面版会把工坊模块装在：

```text
%APPDATA%\lumina-electron\workshop\modules\<moduleId>\<version>\
```

这是写用户配置目录，必须先经用户同意。

## 6. 常见驳回原因（已在本仓库提前规避）

- 包里多了文件（`tools/`、`tests/`、源码工程文件）→ 打包脚本只放白名单文件。
- `ui/index.html` 引用外部资源或用了 `eval` → 打包前静态扫描。
- 图标带 EXIF/ICC、或不是静态 PNG → 图标生成器与校验器都会拦。
- 资产重新上传导致字节变化 → 构建固定时间戳/权限/顺序，两次构建 sha256 一致。
- 记录里的摘要与资产不符 → 记录由脚本从 `dist/` 现场计算，不手抄。
- 许可证不完整 → 1.3.3 起包内 LICENSE 保留 MIT，并附 SDK（GPL-3.0-or-later）声明与完整 GPLv3。

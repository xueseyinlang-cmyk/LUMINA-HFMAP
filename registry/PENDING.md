# 发布状态（1.3.3）

模块 `studio.heightforge.heightmap`（HeightForge 高度图）的已发布版本与 Registry 提交状态。

## 已发布（不可变资产）

| 版本 | tag | 资产 | 字节 | 包 sha256 | manifest sha256 |
| --- | --- | --- | --- | --- | --- |
| 1.3.1 | `v1.3.1` | `studio.heightforge.heightmap-1.3.1.lumina-workshop` | 197,712 | `984361f65526f1bd38928e149be0bc717418f6ecd5837111c92623376dcb6232` | `da26be3c722a0d6f907dd45596b1533b12ab50019b2b3ef974f2ca4527bc61e8` |
| 1.3.2 | `v1.3.2` | `studio.heightforge.heightmap-1.3.2.lumina-workshop` | 206,056 | `b62c56cc0b2546873d8b9e5634451e5bc6b7bef3a33c26e7663d1dbed6cd7f20` | `08c45a871656d42237c5839005abca05857805b9d643495f4bd91b4cadf08347` |
| 1.3.3 | `v1.3.3` | `studio.heightforge.heightmap-1.3.3.lumina-workshop` | 219,755 | `24f0fde9ad26d41630878e5a0ff959304db59e4a9e7a90d241cb205006830459` | `a64d2740fa9b637a3e3aed780a1199409b7cccd5263fa804b8ecb75d93080ada` |

1.3.3 是许可证合规版本：包内 LICENSE 保留项目 MIT 条款，新增内联
`@lumina/workshop-sdk` 1.0.2（GPL-3.0-or-later）的版权/许可声明，并附 GNU GPLv3 完整文本；
代码与渲染逻辑零改动。

## Registry 提交

- PR #12：`feat(modules): register studio.heightforge.heightmap 1.3.2 / 1.3.3`
  - 上游：`lumina-layer-studio/Lumina-Workshop-Registry`
  - head：`xueseyinlang-cmyk:add/studio.heightforge.heightmap-1.3.2`（`5535d5ba`）
  - 记录 `versions`：1.3.1 / 1.3.2 / 1.3.3（只追加，历史条目未改）
  - 状态：open、mergeable、check-runs 0（status pending，等待维护者触发/审核）
  - 记录校验：`validate_record` 通过

## 合并之后

1. 维护者在受保护环境签名并重建 Registry 目录；
2. 1.3.3 出现在官方目录后，客户端可正常获取；
3. 已发布资产不可变，1.3.1 / 1.3.2 保持原样。

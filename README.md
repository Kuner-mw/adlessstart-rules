# adlessstart-rules

清启动（AdlessStart）生态的规则与数据仓库：存放做好的组件标注规则，后续会继续补充 IP 名单等数据。

## 当前内容

| 路径 | 说明 | 条数 |
|---|---|---|
| [`component-rules/merged_annotations.json`](component-rules/merged_annotations.json) | 组件标注合并库：四源合并去重、全量中文、六档标签、带禁用后果说明 | 817 |

数据由以下四个公开来源合并而成（详见各来源自身的许可与说明）：

- [Blocker 通用规则](https://github.com/lihenggui/blocker-general-rules)（中/英文，512 条 × 2，按 id 配对去重）
- [Exodus Privacy 追踪器库](https://reports.exodus-privacy.eu.org)（428 个追踪器，423 个与 Blocker 重叠归并）
- [LibChecker 组件规则库](https://github.com/LibChecker/LibChecker-Rules)（rules-data v50，type 1-5 原始 1,296 条，去重后 1,274 条规则）

合并口径：归一化关键字（小写、去尾点）完全相同才归并，中文字段优先保留；LibChecker 类名规则按 label 归组后并入对应 SDK 组，剩余按完全公共包前缀归并；同名兜底 + 人工并组。

## 六档标签

每条记录的 `tag` 取以下六档之一，判定标准：

| 标签 | 判定标准 | 条数 |
|---|---|---|
| **必要基础** | 缺了会崩溃或无法运行的框架级库（Jetpack/Kotlin/OkHttp/图片加载/数据库/跨平台运行时等），**不宜禁用** | 149 |
| **广告** | 广告、追踪、营销、归因组件——组件管理的拦截候选 | 306 |
| **监控** | APM、崩溃上报、统计分析、受众测量、遥测等数据采集观测类，禁用不影响运行 | 138 |
| **功能组件** | 可选业务功能：地图、支付、登录、分享、IM/客服、扫码、安全风控、媒体播放、小程序容器等 | 155 |
| **厂商推送** | 手机厂商系统推送通道（小米/华为/荣耀/OPPO/vivo/魅族/nubia）及其代理 | 34 |
| **其它推送** | 第三方推送通道（极光、个推、信鸽、FCM、OneSignal 等） | 35 |

所有条目均带 `sideEffect` 字段（禁用后果说明，源数据缺失时按档位补写）。

## 字段说明

每条记录：

```json
{
  "name": "穿山甲广告SDK",
  "company": "北京巨量引擎网络技术有限公司",
  "description": "……",
  "sideEffect": "……（禁用后果）",
  "safeToBlock": true,
  "tag": "广告",
  "keywords": ["com.byted.pangle."],
  "regexes": [],
  "networkSignatures": [],
  "sources": ["blocker_zh", "exodus", "libchecker"],
  "id": "sdk0495"
}
```

- 核心匹配字段：`keywords`（类名前缀，带尾点为纯前缀）/ `regexes`（正则）/ `networkSignatures`（域名）
- `tag` / `sideEffect` / `sources` / `id` 为本仓库附加字段，解析器不识别会自动忽略
- `safeToBlock` 仅在确有依据时出现（`true` / `false`），未知时省略键

## 作为清启动自定义规则源使用

App 内「规则源 → 添加自定义源」填入：

```
https://raw.githubusercontent.com/Kuner-mw/adlessstart-rules/main/component-rules/merged_annotations.json
```

## 计划中

- `ip/`：IP 名单（占位目录，后续添加）

## 许可

数据来自上述社区项目，各自的原始许可继续适用；本仓库整理部分的许可待定。

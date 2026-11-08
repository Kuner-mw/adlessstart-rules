# adlessstart-rules

清启动（AdlessStart）生态的规则与数据仓库：存放做好的组件标注规则，后续会继续补充 IP 名单等数据。

## 当前内容

| 路径 | 说明 | 条数 |
|---|---|---|
| [`component-rules/merged_annotations.json`](component-rules/merged_annotations.json) | 组件标注合并库：四源合并去重、全量中文、带标签 | 817（广告 401 / 基础组件 416） |

数据由以下四个公开来源合并而成（详见各来源自身的许可与说明）：

- [Blocker 通用规则](https://github.com/lihenggui/blocker-general-rules)（中/英文，512 条 × 2，按 id 配对去重）
- [Exodus Privacy 追踪器库](https://reports.exodus-privacy.eu.org)（428 个追踪器，423 个与 Blocker 重叠归并）
- [LibChecker 组件规则库](https://github.com/LibChecker/LibChecker-Rules)（rules-data v50，type 1-5 原始 1,296 条，去重后 1,274 条规则）

合并口径：归一化关键字（小写、去尾点）完全相同才归并，中文字段优先保留；LibChecker 类名规则按 label 归组后并入对应 SDK 组，剩余按完全公共包前缀归并；同名兜底 + 人工并组。

## 字段说明

每条记录：

```json
{
  "name": "穿山甲广告SDK",
  "company": "北京巨量引擎网络技术有限公司",
  "description": "……",
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
- `tag`（`广告` / `基础组件`）、`sources`（数据来源）、`id` 为本仓库附加字段，解析器不识别会自动忽略
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

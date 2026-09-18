# 设计记录：四端基础分流模板

日期：2026-09-16（2026-09-18 增加 Shadowrocket）

## 目标

给「机场只给节点不给规则」的用户一套开箱即用的 Shadowrocket / Surge / Clash Meta / Stash 分流模板。
用户只需替换一行订阅地址。

## 决策

| 决策 | 选择 | 原因 |
|---|---|---|
| Clash 内核 | mihomo（Clash Meta） | 主流客户端都已切换到 mihomo；需要 `filter`、`format: text`、`REJECT-DROP` |
| 订阅接入 | 下载模板 + 替换占位符 | 三端一致，不依赖客户端特性（如 Stash override） |
| 地区分组 | Surge `smart`，Clash / Stash `url-test` | Stash 与 mihomo 主线不支持 `smart` |
| 个人规则 | 全部剔除 | 模板只保留通用分流，用户在规则区开头自加 |
| 广告拦截 | 启用，指向 `⛔️ 广告拦截` 组 | 用户可一键切到直连关闭 |
| 规则源 | Sukka ruleset.skk.moe 为主，blackmatrix7 补 YouTube / Google | 维护活跃；Sukka 提供 Clash 专用格式，Stash 可直接复用 |
| GitHub 托管规则 | 走 jsDelivr CDN | 国内可达性 |
| 局域网共享 | Surge 的 `http-listen` / `socks5-listen` 默认注释 | 公开模板不应默认对局域网开放代理 |
| Stash 与 Clash 分文件 | 是 | Stash 的 `type` 字段仅用于 `inline`，mihomo 的 `rule-providers` 必须 `type: http` |
| Shadowrocket 不放订阅占位符 | 是 | 小火箭订阅在首页单独管理，`policy-regex-filter` 无 `policy-path` 时直接作用于全部节点，用户零修改 |
| Shadowrocket 规则源 | 全部换 blackmatrix7 `rule/Shadowrocket/` | Sukka README 明确不支持也不会支持 Shadowrocket；半官方手册（LOWERTOP）也用 blackmatrix7 |
| Shadowrocket `🟢 Proxy` 多一个 `PROXY` | 是 | `PROXY` = 首页选中节点，照顾小火箭用户习惯；默认仍是 `♻️ 自动选择` |
| Shadowrocket 广告规则 | `AdvertisingLite`（3.8 万域名） | 完整版 `Advertising` 28 万域名，iOS 内存和误杀都不友好；README 说明如何换 |
| README 顺序 | Shadowrocket 第一 | 用户量最大且用法最简单 |

## 规则集格式对照

| 类型 | Surge | Clash / Stash | Shadowrocket（blackmatrix7） |
|---|---|---|---|
| domainset | `DOMAIN-SET,.../List/domainset/x.conf` | `behavior: domain`, `format: text`, `.../Clash/domainset/x.txt` | `DOMAIN-SET,.../rule/Shadowrocket/X/X_Domain.list` |
| non_ip / ip | `RULE-SET,.../List/non_ip/x.conf` | `behavior: classical`, `format: text`, `.../Clash/non_ip/x.txt` | `RULE-SET,.../rule/Shadowrocket/X/X.list` |

## Shadowrocket 规则映射

| 模板分组 | blackmatrix7 规则 |
|---|---|
| 📺 谷歌服务 | YouTube、Google |
| ⛔️ 广告拦截 | AdvertisingLite（+ _Domain）、Privacy（+ _Domain）、Hijacking |
| 🟢 Proxy | Speedtest、Download、GitHub、Proxy（+ _Domain） |
| 🍎 苹果服务 | iCloud、Apple（+ _Domain） |
| Ⓜ️ 微软服务 | Microsoft |
| 🌍 国外媒体 | GlobalMedia（+ _Domain） |
| 📲 电报信息 | Telegram |
| 🤖 AIGC | OpenAI、Claude、Gemini、Copilot |
| DIRECT | Direct、China（+ _Domain）、Lan、GEOIP CN |

省略 `PROCESS-NAME`（iOS 无进程匹配）、`extended-matching` / `dns-failed`（Surge 专有参数）。

## 剔除的原配置内容

- 单条 `DOMAIN-SUFFIX` / `PROCESS-NAME` 个人规则
- broker-rules / Topstep 券商规则及 `💹 券商` 组（作为可选项在 README 里给出加回方法）
- 无规则引用的 `🚫 运营劫持`、`🌏 国内媒体` 组
- 个人 gist 规则、raw.githubusercontent 源的 Discord 规则
- 注释掉的分地区流媒体规则

# proxy-config-templates

Surge / Clash Meta (mihomo) / Stash 三端通用的**基础分流配置模板**。

现在很多机场只给节点、不给规则，拿到订阅之后还得自己折腾分流。这个仓库提供一套开箱即用的模板：**下载 → 改一行订阅地址 → 导入客户端**，就能得到广告拦截、流媒体、AI、Telegram、苹果 / 微软服务、地区自动选择等常用分流。

三个客户端的策略组命名完全一致，换客户端不用重新适应。

## 文件说明

| 客户端 | 文件 | 适用 App | 下载链接 |
|---|---|---|---|
| Surge 5 | [`surge/Surge.conf`](surge/Surge.conf) | Surge for Mac / iOS | [jsDelivr](https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/surge/Surge.conf) |
| Clash Meta | [`clash/Clash.yaml`](clash/Clash.yaml) | Clash Verge Rev、ClashX Meta、Clash Meta for Android、FlClash 等 mihomo 内核客户端 | [jsDelivr](https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/clash/Clash.yaml) |
| Stash | [`stash/Stash.yaml`](stash/Stash.yaml) | Stash for iOS / macOS / tvOS | [jsDelivr](https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/stash/Stash.yaml) |

## 使用方法

三个模板的用法都一样，只有一步是必须的：**把占位符换成你的机场订阅链接**。

模板里的占位符是：

```
https://example.com/YOUR_SUBSCRIPTION_URL
```

### Surge

1. 下载 `surge/Surge.conf`
2. 用文本编辑器打开，找到 `[Proxy Group]` 里的这一行：
   ```
   ✈️ 机场 = select, policy-path=https://example.com/YOUR_SUBSCRIPTION_URL, update-interval=86400, include-all-proxies=0
   ```
   把 `https://example.com/YOUR_SUBSCRIPTION_URL` 换成你的订阅链接
3. 导入：
   - **Mac**：把文件放到 `~/Library/Application Support/Surge/Profiles/`（或直接双击 `.conf` 文件），然后在 Surge 菜单里选择这个配置
   - **iOS**：通过 iCloud Drive 把文件放到 `Surge` 目录，或者在 Surge → 配置 → 从 iCloud / 文件导入

> 机场订阅需要支持 Surge 格式（`policy-path` 拉取的是 Surge 节点列表）。绝大多数机场会根据 User-Agent 自动返回对应格式，如果不行，请到机场后台找「Surge 订阅」链接。

### Clash Meta (mihomo)

1. 下载 `clash/Clash.yaml`
2. 找到 `proxy-providers` 段：
   ```yaml
   proxy-providers:
     airport:
       type: http
       url: "https://example.com/YOUR_SUBSCRIPTION_URL"
   ```
   把 `url` 换成你的订阅链接（需要 **Clash 格式**的订阅）
3. 导入：
   - **Clash Verge Rev**：订阅 → 新建 → 类型选「本地」→ 选择文件
   - **ClashX Meta**：把文件放到 `~/.config/clash/`，菜单栏 → 配置 → 选择
   - **Clash Meta for Android / FlClash**：配置 → 新建 → 文件 → 选择

> 模板用了 `filter`、`format: text`、`REJECT-DROP` 等 mihomo 特性，**原版 Clash Premium 内核不兼容**。

### Stash

1. 下载 `stash/Stash.yaml`
2. 和 Clash 一样，把 `proxy-providers` 里的 `url` 换成你的订阅链接（Clash 格式）
3. 导入：Stash → 配置 → 右上角「+」→ 从文件 / iCloud 导入

## 策略组说明

| 策略组 | 默认 | 说明 |
|---|---|---|
| `🟢 Proxy` | ♻️ 自动选择 | 总出口，其它分组最终都会落到这里 |
| `♻️ 自动选择` | — | 从六个地区的节点里自动选延迟最低的（Surge 用 `smart`，Clash / Stash 用 `url-test`） |
| `🇭🇰 香港` `🇺🇸 美国` `🇯🇵 日本` `🇸🇬 新加坡` `🇰🇷 韩国` `🇨🇳 台湾` | — | 按节点名正则筛出对应地区，自动测速 |
| `⛔️ 广告拦截` | REJECT | 广告 / 隐私跟踪 / 钓鱼域名。不想拦截就切到「🚀 直接连接」 |
| `🌍 国外媒体` | 🟢 Proxy | Netflix、Disney+、Spotify 等 |
| `📺 谷歌服务` | 🇰🇷 韩国 | YouTube + 其它 Google 服务固定走一个地区，避免 YouTube Premium 地区漂移 |
| `🤖 AIGC` | 🇸🇬 新加坡 | OpenAI / Claude / Gemini 等，选一个 AI 服务支持的地区 |
| `📲 电报信息` | 🟢 Proxy | Telegram |
| `Ⓜ️ 微软服务` | 🟢 Proxy | Office、OneDrive、GitHub 等 |
| `🍎 苹果服务` | 🚀 直接连接 | App Store、iCloud 等，国内直连即可 |
| `🐟 漏网之鱼` | 🟢 Proxy | 没有匹配到任何规则的流量 |
| `✈️ 机场` | — | 机场订阅本身，一般不用手动选它 |

## 规则来源

规则全部来自公开维护的规则集，客户端会定期自动更新：

- [Sukka's Ruleset (ruleset.skk.moe)](https://github.com/SukkaW/Surge) — 广告拦截、CDN、流媒体、Telegram、AI、苹果 / 微软、国内外域名、IP 段（主力）
- [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) — YouTube、Google
- [soffchen/GeoIP2-CN](https://github.com/soffchen/GeoIP2-CN) — Surge 用的精简 GeoIP 库（只含中国 IP 段）

感谢以上项目的维护者。

## 自定义

**加个人规则**：在规则区开头添加，越靠前优先级越高。三个文件里都留了注释标记的位置。

```
# Surge
DOMAIN-SUFFIX,example.com,"🟢 Proxy"

# Clash / Stash
- DOMAIN-SUFFIX,example.com,🟢 Proxy
```

**改地区默认选项**：直接在客户端界面里切换即可，客户端会记住你的选择（Clash 模板已开启 `store-selected`）。

**共享代理给局域网设备**（仅 Surge Mac）：把 `[General]` 里这两行的注释去掉：

```
# http-listen = 0.0.0.0
# socks5-listen = 0.0.0.0
```

## 可选：券商分流

炒港美股的用户可以加上 [forecho/broker-rules](https://github.com/forecho/broker-rules)，把富途、长桥、老虎、嘉信、雪球、华盛通、复星等券商 App 的流量固定到一个地区（一般走香港或新加坡，避免出口地区漂移触发风控）。模板默认不包含，按下面的方式加上即可，规则放在**规则区开头**（和个人规则同一位置），保证优先于 CDN / 海外域名等通用规则。

### Surge

`[Proxy Group]` 加一个分组：

```
💹 券商 = select, "🇭🇰 香港", "🇸🇬 新加坡", "🇺🇸 美国", "🟢 Proxy"
```

`[Rule]` 开头加一行：

```
RULE-SET,https://cdn.jsdelivr.net/gh/forecho/broker-rules@main/rule/Surge/Broker.list,"💹 券商"
```

### Clash Meta (mihomo)

```yaml
proxy-groups:
  - name: 💹 券商
    type: select
    proxies:
      - 🇭🇰 香港
      - 🇸🇬 新加坡
      - 🇺🇸 美国
      - 🟢 Proxy

rule-providers:
  broker:
    type: http
    behavior: classical
    format: yaml
    url: https://cdn.jsdelivr.net/gh/forecho/broker-rules@main/rule/Clash/Broker.yaml
    path: ./rules/broker.yaml
    interval: 86400

rules:
  - RULE-SET,broker,💹 券商
```

### Stash

和 Clash 一样，只是 `rule-providers` 不写 `type: http`，URL 换成 Stash 版本：

```yaml
rule-providers:
  broker:
    behavior: classical
    format: yaml
    url: https://cdn.jsdelivr.net/gh/forecho/broker-rules@main/rule/Stash/Broker.yaml
    path: ./rules/broker.yaml
    interval: 86400
```

> broker-rules 还单独提供了 [Topstep](https://github.com/forecho/broker-rules#怎么用) 期货规则，用法相同，一般配成直连。

## 可选：银行直连

众安银行、HSBC HK、大象银行、Wise 这类银行 App 会检测客户端 IP，走代理会直接登不上或者被风控，需要固定直连。模板默认不包含，按下面的方式加上，同样放在**规则区开头**。

规则来自 [yeahwu/Rules-For-Quantumult-X](https://github.com/yeahwu/Rules-For-Quantumult-X/blob/main/Rules/Services/Bank.list)。上游是 Quantumult X 格式（`HOST` / `HOST-SUFFIX`），Surge / Clash / Stash 都不识别，没法直接订阅，所以下面已经转成 `DOMAIN` / `DOMAIN-SUFFIX` 内联写法，上游有更新需要手动同步。

> 列表里混了几个通用 SDK 域名（`crashlytics.com`、`app-measurement.com`、`appsflyersdk.com`、`cdn.optimizely.com`、`api.mixpanel.com` 等），这些是银行 App 启动时会连的埋点 / 反欺诈服务，走代理同样可能触发风控，所以一并直连。副作用是其它 App 的埋点上报也会直连，基本无感。

### Surge

`[Proxy Group]` 加一个分组，第一项 `🚀 直接连接` 就是默认值：

```
🏦 银行 = select, "🚀 直接连接", "🇭🇰 香港", "🟢 Proxy"
```

`[Rule]` 开头加：

```
DOMAIN,c-hsbc.lytics.io,"🏦 银行"
DOMAIN,mobile.eum-appdynamics.com,"🏦 银行"
DOMAIN,tags.tiqcdn.com,"🏦 银行"
DOMAIN,hsbc.edge.sdk.awswaf.com,"🏦 银行"
DOMAIN,cdnbc.wup.hsbc.com.hk,"🏦 银行"
DOMAIN,cdn.optimizely.com,"🏦 银行"
DOMAIN,log-58144bf0.we-stats.com,"🏦 银行"
DOMAIN-SUFFIX,tealiumiq.com,"🏦 银行"
DOMAIN-SUFFIX,hsbc.com.hk,"🏦 银行"
DOMAIN-SUFFIX,online-metrix.net,"🏦 银行"
DOMAIN-SUFFIX,cloud1.vv1865.com,"🏦 银行"
DOMAIN-SUFFIX,liveperson.net,"🏦 银行"
DOMAIN-SUFFIX,lpsnmedia.net,"🏦 银行"
DOMAIN-SUFFIX,elebank.com,"🏦 银行"
DOMAIN-SUFFIX,appsflyersdk.com,"🏦 银行"
DOMAIN-SUFFIX,za.group,"🏦 银行"
DOMAIN-SUFFIX,zainvest.group,"🏦 银行"
DOMAIN-SUFFIX,crashlytics.com,"🏦 银行"
DOMAIN-SUFFIX,zajourney.com,"🏦 银行"
DOMAIN,app-measurement.com,"🏦 银行"
DOMAIN,api.mixpanel.com,"🏦 银行"
DOMAIN-SUFFIX,wise.com,"🏦 银行"
```

### Clash Meta (mihomo) / Stash

两者写法相同：

```yaml
proxy-groups:
  - name: 🏦 银行
    type: select
    proxies:
      - 🚀 直接连接
      - 🇭🇰 香港
      - 🟢 Proxy

rules:
  - DOMAIN,c-hsbc.lytics.io,🏦 银行
  - DOMAIN,mobile.eum-appdynamics.com,🏦 银行
  - DOMAIN,tags.tiqcdn.com,🏦 银行
  - DOMAIN,hsbc.edge.sdk.awswaf.com,🏦 银行
  - DOMAIN,cdnbc.wup.hsbc.com.hk,🏦 银行
  - DOMAIN,cdn.optimizely.com,🏦 银行
  - DOMAIN,log-58144bf0.we-stats.com,🏦 银行
  - DOMAIN-SUFFIX,tealiumiq.com,🏦 银行
  - DOMAIN-SUFFIX,hsbc.com.hk,🏦 银行
  - DOMAIN-SUFFIX,online-metrix.net,🏦 银行
  - DOMAIN-SUFFIX,cloud1.vv1865.com,🏦 银行
  - DOMAIN-SUFFIX,liveperson.net,🏦 银行
  - DOMAIN-SUFFIX,lpsnmedia.net,🏦 银行
  - DOMAIN-SUFFIX,elebank.com,🏦 银行
  - DOMAIN-SUFFIX,appsflyersdk.com,🏦 银行
  - DOMAIN-SUFFIX,za.group,🏦 银行
  - DOMAIN-SUFFIX,zainvest.group,🏦 银行
  - DOMAIN-SUFFIX,crashlytics.com,🏦 银行
  - DOMAIN-SUFFIX,zajourney.com,🏦 银行
  - DOMAIN,app-measurement.com,🏦 银行
  - DOMAIN,api.mixpanel.com,🏦 银行
  - DOMAIN-SUFFIX,wise.com,🏦 银行
```

## 常见问题

**地区分组里没有节点 / 节点少了？**

地区分组是用正则匹配节点名筛出来的，比如香港用的是 `HongKong|HK|香港|🇭🇰`。如果你的机场节点命名比较特别（比如只写「HKG」），改一下对应分组的正则即可：

- Surge：`policy-regex-filter=...`
- Clash / Stash：`filter: "..."`

**订阅拉不下来？**

- Surge 需要 Surge 格式的节点列表，Clash / Stash 需要 Clash 格式的订阅
- 有些机场订阅链接后面需要加参数指定格式，比如 `&flag=surge` 或 `&flag=clash`，请以机场文档为准

**为什么 Surge 用 `smart`，Clash / Stash 用 `url-test`？**

`smart` 是 Surge 5 独有的策略类型，会综合延迟、丢包和历史表现选节点。Stash 和 mihomo 主线版本都没有这个类型，所以用 `url-test`（纯延迟最低）代替。

**广告拦截误杀了某个网站？**

把 `⛔️ 广告拦截` 切到 `🚀 直接连接` 临时关掉，然后在规则区开头加一条 `DOMAIN-SUFFIX,被误杀的域名,DIRECT` 放行。

## License

[MIT](LICENSE)

# proxy-config-templates

Shadowrocket / Surge / Clash Meta (mihomo) / Stash 四端通用的**基础分流配置模板**。

现在很多机场只给节点、不给规则，拿到订阅之后还得自己折腾分流。这个仓库提供一套开箱即用的模板，直接就能得到广告拦截、流媒体、AI、Telegram、苹果 / 微软服务、地区自动选择等常用分流。

四个客户端的策略组命名完全一致，换客户端不用重新适应。

![Surge for Mac 里的策略组效果](https://r2.imgant.com/2026/09/18/abd22193.png)

> 上图是作者自己 Surge 里的效果，比模板多了券商、银行等几个私人分组；模板里的 `♻️ 自动选择` 对应图中的 `smart Group`。

## 赞助

<table>
<tbody>
<tr>
<td width="180"><a href="https://vip.dd8008.com/"><img src="./assets/duoduoyun.png" alt="朵朵云加速" width="150"></a></td>
<td>感谢 <a href="https://vip.dd8008.com/">朵朵云加速</a> 对本项目的赞助！朵朵云加速是一家主打<b>快速稳定</b>的机场，订阅地址填进本仓库的模板即可直接使用，不用再自己折腾分流。 👉 <a href="https://vip.dd8008.com/">https://vip.dd8008.com/</a></td>
</tr>
</tbody>
</table>

## 文件说明

| 客户端 | 文件 | 适用 App | 下载链接 |
|---|---|---|---|
| Shadowrocket | [`shadowrocket/Shadowrocket.conf`](shadowrocket/Shadowrocket.conf) | Shadowrocket for iOS / macOS | [jsDelivr](https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/shadowrocket/Shadowrocket.conf) |
| Surge 5 | [`surge/Surge.conf`](surge/Surge.conf) | Surge for Mac / iOS | [jsDelivr](https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/surge/Surge.conf) |
| Clash Meta | [`clash/Clash.yaml`](clash/Clash.yaml) | Clash Verge Rev、ClashX Meta、Clash Meta for Android、FlClash 等 mihomo 内核客户端 | [jsDelivr](https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/clash/Clash.yaml) |
| Stash | [`stash/Stash.yaml`](stash/Stash.yaml) | Stash for iOS / macOS / tvOS | [jsDelivr](https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/stash/Stash.yaml) |

## 使用方法

### Shadowrocket（不用改任何东西）

Shadowrocket 把「订阅」和「配置」分开管理，模板会自动从你首页的所有节点里按名字筛出各地区，所以**不需要编辑文件、不需要填订阅地址**：

1. **首页** → 右上角「+」→ 添加你的机场订阅（如果已经加过，跳过）
2. **配置** → 右上角「+」→ 粘贴下面的链接 → 下载
   ```
   https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/shadowrocket/Shadowrocket.conf
   ```
3. 点击刚下载的配置 → **使用配置**
4. **首页** → 全局路由 选「**配置**」

也可以把下面这行复制到 Safari 地址栏打开，一键完成第 2 步：

```
shadowrocket://config/add/https://cdn.jsdelivr.net/gh/forecho/proxy-config-templates@main/shadowrocket/Shadowrocket.conf
```

> 以后模板有更新，在 配置 → 点击该配置 → **更新** 即可拉取最新版；规则集本身会在「使用配置」时自动更新。

---

下面三个客户端需要把模板里的占位符换成你的机场订阅链接，只改这一行：

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
| `🟢 Proxy` | ♻️ 自动选择 | 总出口，其它分组最终都会落到这里。Shadowrocket 版多一个 `PROXY` 选项，表示首页当前选中的节点 |
| `♻️ 自动选择` | — | 从六个地区的节点里自动选延迟最低的（Surge 用 `smart`，其它用 `url-test`） |
| `🇭🇰 香港` `🇺🇸 美国` `🇯🇵 日本` `🇸🇬 新加坡` `🇰🇷 韩国` `🇨🇳 台湾` | — | 按节点名正则筛出对应地区，自动测速 |
| `⛔️ 广告拦截` | REJECT | 广告 / 隐私跟踪 / 钓鱼域名。不想拦截就切到「🚀 直接连接」 |
| `🌍 国外媒体` | 🟢 Proxy | Netflix、Disney+、Spotify 等 |
| `📺 谷歌服务` | 🇰🇷 韩国 | YouTube + 其它 Google 服务固定走一个地区，避免 YouTube Premium 地区漂移 |
| `🤖 AIGC` | 🇸🇬 新加坡 | OpenAI / Claude / Gemini 等，选一个 AI 服务支持的地区 |
| `📲 电报信息` | 🟢 Proxy | Telegram |
| `Ⓜ️ 微软服务` | 🟢 Proxy | Office、OneDrive 等 |
| `🍎 苹果服务` | 🚀 直接连接 | App Store、iCloud 等，国内直连即可 |
| `🐟 漏网之鱼` | 🟢 Proxy | 没有匹配到任何规则的流量 |
| `✈️ 机场` | — | 机场订阅本身，一般不用手动选它（Shadowrocket 版没有这个组，节点在首页管理） |

## 规则来源

规则全部来自公开维护的规则集，客户端会定期自动更新：

- [Sukka's Ruleset (ruleset.skk.moe)](https://github.com/SukkaW/Surge) — Surge / Clash / Stash 版的主力：广告拦截、CDN、流媒体、Telegram、AI、苹果 / 微软、国内外域名、IP 段
- [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) — Surge / Clash / Stash 版的 YouTube、Google；**Shadowrocket 版的全部规则**（Sukka 的规则集不支持 Shadowrocket，所以换用 blackmatrix7 专为它构建的版本）
- [soffchen/GeoIP2-CN](https://github.com/soffchen/GeoIP2-CN) — Surge 用的精简 GeoIP 库（只含中国 IP 段）

感谢以上项目的维护者。

## 自定义

**加个人规则**：在规则区开头添加，越靠前优先级越高。每个文件里都留了注释标记的位置。

```
# Shadowrocket / Surge
DOMAIN-SUFFIX,example.com,🟢 Proxy

# Clash / Stash
- DOMAIN-SUFFIX,example.com,🟢 Proxy
```

Shadowrocket 远程配置改完本地会被「更新」覆盖，建议在 配置 → 编辑配置 里改，或者 fork 本仓库后改自己的链接。

**改地区默认选项**：直接在客户端界面里切换即可，客户端会记住你的选择（Clash 模板已开启 `store-selected`）。

**Shadowrocket 换完整版广告规则**：把 `[Rule]` 里两行 `AdvertisingLite` 换成 `Advertising`，域名数从 3.8 万增加到 28 万，拦得更狠但更吃内存、误杀也更多。

**共享代理给局域网设备**（仅 Surge Mac）：把 `[General]` 里这两行的注释去掉：

```
# http-listen = 0.0.0.0
# socks5-listen = 0.0.0.0
```

## 可选：券商分流

炒港美股的用户可以加上 [forecho/broker-rules](https://github.com/forecho/broker-rules)，把富途、长桥、老虎、嘉信、雪球、华盛通、复星等券商 App 的流量固定到一个地区（一般走香港或新加坡，避免出口地区漂移触发风控）。模板默认不包含，按下面的方式加上即可，规则放在**规则区开头**（和个人规则同一位置），保证优先于 CDN / 海外域名等通用规则。

### Shadowrocket

`[Proxy Group]` 加一个分组：

```
💹 券商 = select,🇭🇰 香港,🇸🇬 新加坡,🇺🇸 美国,🟢 Proxy
```

`[Rule]` 开头加一行：

```
RULE-SET,https://cdn.jsdelivr.net/gh/forecho/broker-rules@main/rule/Shadowrocket/Broker.list,💹 券商
```

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

众安银行、HSBC HK、大象银行、Wise 这类银行 App 会检测客户端 IP，走代理会直接登不上或者被风控，需要固定直连。[forecho/broker-rules](https://github.com/forecho/broker-rules) 提供了单独的 Bank 规则集（源自 [yeahwu/Rules-For-Quantumult-X](https://github.com/yeahwu/Rules-For-Quantumult-X/blob/main/Rules/Services/Bank.list)），模板默认不包含，接入方式和券商一样，同样放在**规则区开头**。

> 规则里包含几个银行 App 会连的通用 SDK 域名（`crashlytics.com`、`app-measurement.com`、`appsflyersdk.com` 等），走代理同样可能触发风控，所以一并直连。副作用是其它 App 的埋点上报也会直连，基本无感。

### Shadowrocket

`[Proxy Group]` 加一个分组，第一项 `🚀 直接连接` 就是默认值：

```
🏦 银行 = select,🚀 直接连接,🇭🇰 香港,🟢 Proxy
```

`[Rule]` 开头加一行：

```
RULE-SET,https://cdn.jsdelivr.net/gh/forecho/broker-rules@main/rule/Shadowrocket/Bank.list,🏦 银行
```

### Surge

`[Proxy Group]` 加一个分组，第一项 `🚀 直接连接` 就是默认值：

```
🏦 银行 = select, "🚀 直接连接", "🇭🇰 香港", "🟢 Proxy"
```

`[Rule]` 开头加一行：

```
RULE-SET,https://cdn.jsdelivr.net/gh/forecho/broker-rules@main/rule/Surge/Bank.list,"🏦 银行"
```

### Clash Meta (mihomo)

```yaml
proxy-groups:
  - name: 🏦 银行
    type: select
    proxies:
      - 🚀 直接连接
      - 🇭🇰 香港
      - 🟢 Proxy

rule-providers:
  bank:
    type: http
    behavior: classical
    format: yaml
    url: https://cdn.jsdelivr.net/gh/forecho/broker-rules@main/rule/Clash/Bank.yaml
    path: ./rules/bank.yaml
    interval: 86400

rules:
  - RULE-SET,bank,🏦 银行
```

### Stash

和 Clash 一样，只是 `rule-providers` 不写 `type: http`，URL 换成 Stash 版本：

```yaml
rule-providers:
  bank:
    behavior: classical
    format: yaml
    url: https://cdn.jsdelivr.net/gh/forecho/broker-rules@main/rule/Stash/Bank.yaml
    path: ./rules/bank.yaml
    interval: 86400
```

## 常见问题

**地区分组里没有节点 / 节点少了？**

地区分组是用正则匹配节点名筛出来的，比如香港用的是 `HongKong|HK|香港|🇭🇰`。如果你的机场节点命名比较特别（比如只写「HKG」），改一下对应分组的正则即可：

- Shadowrocket / Surge：`policy-regex-filter=...`
- Clash / Stash：`filter: "..."`

**Shadowrocket 里 `PROXY` 是什么？**

Shadowrocket 内置的策略，等于首页当前选中的那个节点。习惯"首页点一个节点就用"的话，把 `🟢 Proxy` 切到 `PROXY` 即可。

**Shadowrocket 提示规则集下载失败？**

blackmatrix7 的仓库很大，jsDelivr 第一次抓取偶尔会失败。到 配置 → 编辑配置 → 规则集 URL 里点一下失败的地址重试，或者再点一次「使用配置」。

**订阅拉不下来？**

- Surge 需要 Surge 格式的节点列表，Clash / Stash 需要 Clash 格式的订阅
- 有些机场订阅链接后面需要加参数指定格式，比如 `&flag=surge` 或 `&flag=clash`，请以机场文档为准

**为什么 Surge 用 `smart`，其它用 `url-test`？**

`smart` 是 Surge 5 独有的策略类型，会综合延迟、丢包和历史表现选节点。Shadowrocket、Stash 和 mihomo 主线版本都没有这个类型，所以用 `url-test`（纯延迟最低）代替。

**广告拦截误杀了某个网站？**

把 `⛔️ 广告拦截` 切到 `🚀 直接连接` 临时关掉，然后在规则区开头加一条 `DOMAIN-SUFFIX,被误杀的域名,DIRECT` 放行。

## License

[MIT](LICENSE)

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

# Nikki 规则维护说明

这份文档说明 GitHub 仓库里的规则文件，和 `/etc/nikki/profiles/Nikkinew.yaml`、9090 面板里的策略组之间是什么关系。

## 工作方式

一条自定义规则的链路是：

```text
rules/custom/.../*.list
  -> rule-providers 里的 custom_xxx
  -> rules 里的 RULE-SET,custom_xxx,🎯 自定义-xxx
  -> 9090 UI 里手动选择 🎯 自定义-xxx 走哪个节点
```

域名轮询是另一条可选链路：

```text
rules/custom/round-robin/sites.list
  -> rule-providers 里的 custom_round_robin
  -> mixin.yaml 里的 RULE-SET,custom_round_robin,🎲 自定义-域名轮询
  -> 9090 UI 里选择 🎲 轮询-全部 / 🎲 轮询-亚洲 / 🎲 轮询-美国
```

也就是说：

- 想新增匹配规则：改 `rules/custom/.../*.list`
- 想改默认走哪个策略：改 `snippets/custom-proxy-groups.yaml` 里对应组的 `proxies` 顺序
- 想在 9090 UI 里临时切换：直接打开对应的 `🎯 自定义-*` 组选择
- 想让某些域名轮询：改 `rules/custom/round-robin/sites.list`，再把 `snippets/round-robin-mixin.yaml` 贴进 `/etc/nikki/mixin.yaml`

## 自定义规则对应关系

| 规则文件 | Provider | 配置里的规则 | 9090 面板组 | 用途 |
|---|---|---|---|---|
| `rules/custom/direct/trackers.list` | `custom_direct_trackers` | `RULE-SET,custom_direct_trackers,🎯 自定义-Tracker` | `🎯 自定义-Tracker` | PT/BT tracker、需要直连的 tracker/CDN |
| `rules/custom/direct/payments.list` | `custom_direct_payments` | `RULE-SET,custom_direct_payments,🎯 自定义-直连支付` | `🎯 自定义-直连支付` | 明确需要直连的支付网关 |
| `rules/custom/loc/hk-media.list` | `custom_loc_hk_media` | `RULE-SET,custom_loc_hk_media,🎯 自定义-HK媒体` | `🎯 自定义-HK媒体` | TVB、myTVSUPER、香港媒体 |
| `rules/custom/loc/sites.list` | `custom_loc_sites` | `RULE-SET,custom_loc_sites,🎯 自定义-loc站点` | `🎯 自定义-loc站点` | 其它手动指定给 `loc` 的站点 |
| `rules/custom/proxy/ai.list` | `custom_proxy_ai` | `RULE-SET,custom_proxy_ai,🎯 自定义-AI` | `🎯 自定义-AI` | OpenAI、ChatGPT、AI 工具、模型站点 |
| `rules/custom/proxy/dev.list` | `custom_proxy_dev` | `RULE-SET,custom_proxy_dev,🎯 自定义-开发` | `🎯 自定义-开发` | GitHub、Docker、PyPI、OpenWrt、驱动、开发资源 |
| `rules/custom/proxy/media.list` | `custom_proxy_media` | `RULE-SET,custom_proxy_media,🎯 自定义-媒体` | `🎯 自定义-媒体` | TMDB、TVDB、壁纸、媒体资源 |
| `rules/custom/proxy/adult.list` | `custom_proxy_adult` | `RULE-SET,custom_proxy_adult,🎯 自定义-Adult` | `🎯 自定义-Adult` | 成人/特殊媒体站点 |
| `rules/custom/proxy/payments.list` | `custom_proxy_payments` | `RULE-SET,custom_proxy_payments,🎯 自定义-支付` | `🎯 自定义-支付` | Stripe、Gopay、海外支付 API |
| `rules/custom/proxy/social.list` | `custom_proxy_social` | `RULE-SET,custom_proxy_social,🎯 自定义-社交` | `🎯 自定义-社交` | Discord、WhatsApp 等社交通讯 |
| `rules/custom/proxy/misc.list` | `custom_proxy_misc` | `RULE-SET,custom_proxy_misc,🎯 自定义-杂项` | `🎯 自定义-杂项` | 暂时不好分类但需要代理的域名 |
| `rules/custom/round-robin/sites.list` | `custom_round_robin` | `RULE-SET,custom_round_robin,🎲 自定义-域名轮询` | `🎲 自定义-域名轮询` | 需要轮询分配到多个节点的域名 |

## 怎么新增一条规则

先判断它属于哪一类，然后把规则加到对应 `.list` 文件末尾。

常用格式：

```text
DOMAIN,example.com
DOMAIN-SUFFIX,example.com
DOMAIN-KEYWORD,example
IP-CIDR,1.2.3.0/24,no-resolve
```

建议优先级：

- 精确域名用 `DOMAIN,example.com`
- 整个主域和子域都匹配用 `DOMAIN-SUFFIX,example.com`
- 只知道关键词，或者域名变化很多，用 `DOMAIN-KEYWORD,example`
- IP 段用 `IP-CIDR,...,no-resolve`

例子：

```text
# 新增 AI 站点
rules/custom/proxy/ai.list
DOMAIN-SUFFIX,openrouter.ai

# 新增开发下载站
rules/custom/proxy/dev.list
DOMAIN-SUFFIX,nodejs.org

# 新增 PT tracker
rules/custom/direct/trackers.list
DOMAIN,tracker.example.org
```

## 怎么让某一类默认走不同节点

打开 `snippets/custom-proxy-groups.yaml`，找到对应的组，把想默认使用的策略放到 `proxies` 第一位。

例如想让 `🎯 自定义-AI` 默认走美国自动：

```yaml
- name: 🎯 自定义-AI
  type: select
  proxies:
  - ♻️ 美国自动
  - 🤖 ChatGPT
  - 🔯 美国故转
  - 🌐 全部节点
```

如果只是临时切换，不需要改仓库，直接在 9090 UI 里点 `🎯 自定义-AI` 选择即可。

## 怎么开启域名轮询

1. 把 `snippets/round-robin-mixin.yaml` 合并进 `/etc/nikki/mixin.yaml`。
2. 把 `mixin_file_content` 打开成 `1`。
3. 把想轮询的域名加到 `rules/custom/round-robin/sites.list`。
4. 重载 Nikki 后，在 9090 UI 里把 `🎲 自定义-域名轮询` 选成 `🎲 轮询-全部` / `🎲 轮询-亚洲` / `🎲 轮询-美国`。

## 改完仓库后怎么生效

1. 提交并推送到 GitHub。
2. 等 Nikki/mihomo 的 rule-provider 自动更新，或者在面板里手动更新 provider。
3. 如果改的是 `rules/custom/.../*.list`，通常不需要改路由器配置。
4. 如果改的是 `snippets/rules-and-providers.yaml` 或 `snippets/custom-proxy-groups.yaml`，需要同步到 `/etc/nikki/profiles/Nikkinew.yaml` 后重启 Nikki。
5. 如果改的是 `snippets/round-robin-mixin.yaml`，需要同步到 `/etc/nikki/mixin.yaml`，并确保 `mixin_file_content=1`。

## 不要放进公开仓库的内容

不要把这些放进 GitHub：

- 机场订阅地址
- 节点密码、token、uuid
- 内网带 apikey 的接口，例如 `tracker-4` / `tracker-6`
- 完整 `/etc/nikki/run/config.yaml`

这个仓库只维护公开规则和可复用配置片段。

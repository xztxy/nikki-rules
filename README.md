# nikki-rules

集中维护 `/etc/nikki/profiles/Nikkinew.yaml` 使用的规则。

## 目录

- `rules/upstream/domain/`: 原 profile 里的上游 domain `.mrs` 规则镜像。
- `rules/upstream/ip/`: 原 profile 里的上游 ipcidr `.mrs` 规则镜像。
- `rules/upstream/classical/`: 原 profile 里的上游 classical/text 规则镜像。
- `rules/custom/direct/`: 你的自定义直连规则。
- `rules/custom/loc/`: 你的自定义 `loc` 节点规则。
- `rules/custom/proxy/`: 你的自定义代理规则，按 AI、开发、媒体、支付、社交、成人内容和杂项拆分。
- `rules/custom/round-robin/`: 需要节点轮询的可选域名规则。
- `snippets/rules-and-providers.yaml`: 可粘贴进 `Nikkinew.yaml` 的完整 `rules` + `rule-providers` 片段。
- `snippets/custom-proxy-groups.yaml`: 自定义规则分类对应的可选策略组，会显示在 9090 UI 里。
- `snippets/round-robin-mixin.yaml`: 可选的域名轮询 mixin 片段，直接贴进 `/etc/nikki/mixin.yaml`。
- `audit/`: 本次整理的来源和分类审计。

## 自定义分类

- `custom_direct_trackers`: PT/BT tracker 和需要直连的相关域名。
- `custom_direct_payments`: 需要直连的支付网关。
- `custom_loc_hk_media`: TVB / 香港媒体相关规则，走 `loc`。
- `custom_loc_sites`: 其它手动指定走 `loc` 的站点。
- `custom_proxy_ai`: AI、模型、OpenAI 相关规则，走 `🤖 ChatGPT`。
- `custom_proxy_dev`: 开发、包管理、驱动、基础设施相关规则，走 `👨🏿‍💻 GitHub`。
- `custom_proxy_media`: 媒体数据库和资源站，走 `🚀 默认代理`。
- `custom_proxy_adult`: 成人/媒体站点，走 `🚀 默认代理`。
- `custom_proxy_payments`: 海外支付和 API，走 `💶 PayPal`。
- `custom_proxy_social`: 社交和通讯站点，走 `🚀 默认代理`。
- `custom_proxy_misc`: 其它手动代理规则，走 `🚀 默认代理`。
- `custom_round_robin`: 需要按节点轮询的域名，走 `🎲 自定义-域名轮询`。

## 使用方式

在路由器的 `/etc/nikki/profiles/Nikkinew.yaml` 中：

1. 把 `snippets/custom-proxy-groups.yaml` 里的自定义策略组加入原来的 `proxy-groups:`。
2. 用 `snippets/rules-and-providers.yaml` 的内容替换原来的 `rules:`、`rule-anchor:`、`rule-providers:` 三段。
3. 如果要启用域名轮询，把 `snippets/round-robin-mixin.yaml` 贴进 `/etc/nikki/mixin.yaml`，并把 `mixin_file_content` 设成 `1`。

日常新增规则、分类对应关系和 9090 面板组说明见 [MAINTENANCE.md](MAINTENANCE.md)。

这个仓库只保存规则，不保存机场订阅、节点密码或完整路由器配置。

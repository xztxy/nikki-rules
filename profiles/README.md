# Profiles

完整路由器 profile 可能包含订阅或节点信息，所以不提交到仓库。

使用 `snippets/rules-and-providers.yaml` 替换 `/etc/nikki/profiles/Nikkinew.yaml` 里的 `rules:`、`rule-anchor:`、`rule-providers:` 三段。

如果要启用可选的域名轮询，把 `snippets/round-robin-mixin.yaml` 贴进 `/etc/nikki/mixin.yaml`，并把 `mixin_file_content` 打开成 `1`。

本地已生成候选完整配置：`Nikkinew.github-rules.local.yaml`。

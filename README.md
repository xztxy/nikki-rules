# Nikki Custom Rules Export

这些文件是从路由器 `/etc/config/nikki` 中启用的自定义规则导出的，适合放到 GitHub 仓库后用 Nikki `rule_provider` 引用。

## Files

- `custom_direct_classical.txt`: 走 `DIRECT` 的规则
- `custom_loc_classical.txt`: 走 `loc` 的规则
- `custom_global_classical.txt`: 走 `🌐 全部节点` 的规则

## GitHub Raw URL

上传到仓库后，raw 地址通常是：

```text
https://raw.githubusercontent.com/<你的用户名>/<仓库名>/main/nikki/custom_direct_classical.txt
https://raw.githubusercontent.com/<你的用户名>/<仓库名>/main/nikki/custom_loc_classical.txt
https://raw.githubusercontent.com/<你的用户名>/<仓库名>/main/nikki/custom_global_classical.txt
```

如果仓库默认分支是 `master`，把 URL 里的 `main` 改成 `master`。

## Nikki Rule Provider

在 Nikki 插件里新增 3 个规则集：

```text
name: custom_direct
type: http
url: https://raw.githubusercontent.com/<你的用户名>/<仓库名>/main/nikki/custom_direct_classical.txt
behavior: classical
format: text
proxy: GLOBAL
interval: 86400
rule: RULE-SET,custom_direct,DIRECT
```

```text
name: custom_loc
type: http
url: https://raw.githubusercontent.com/<你的用户名>/<仓库名>/main/nikki/custom_loc_classical.txt
behavior: classical
format: text
proxy: GLOBAL
interval: 86400
rule: RULE-SET,custom_loc,loc
```

```text
name: custom_global
type: http
url: https://raw.githubusercontent.com/<你的用户名>/<仓库名>/main/nikki/custom_global_classical.txt
behavior: classical
format: text
proxy: GLOBAL
interval: 86400
rule: RULE-SET,custom_global,🌐 全部节点
```

把这三条 `RULE-SET` 放在通用订阅规则之前、`MATCH` 之前。

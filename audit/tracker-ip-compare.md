# tracker IP 两个版本

当前采用：**版本 A：全网版**，已经合并进 `snippets/rules-and-providers.yaml`。

共享清单：

- `rules/custom/direct/trackers-ip.list`

## 版本 A：全网版

规则：

```text
RULE-SET,custom_direct_trackers_ip,🎯 自定义-Tracker
```

含义：

- 任何设备命中这些 literal IP tracker，都进入同一个 tracker 组
- 适合多台设备都可能跑 qB / TR 的场景

## 版本 B：NAS 限定版

规则：

```text
AND,((SRC-IP-CIDR,192.168.15.161/32),(RULE-SET,custom_direct_trackers_ip)),🎯 自定义-Tracker
```

含义：

- 只让 NAS `192.168.15.161` 命中这些 literal IP tracker
- 适合只想限制 NAS，而不想影响其它设备的场景

## 建议

- 两个版本二选一，不要同时放进主规则
- 真实 tracker IP 追加到 `rules/custom/direct/trackers-ip.list`
- 如果后续 NAS IP 变化，把 `192.168.15.161/32` 一并改掉
- 规则顺序建议放在 `RULE-SET,custom_direct_trackers` 后面

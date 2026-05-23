# qB / Transmission tracker 扫描记录

来源：

- qBittorrent Web API: `192.168.15.161:8083`
- Transmission RPC: `192.168.15.161:9091`

结果：

- 扫到的 tracker 域名里，补进了 `rules/custom/direct/trackers.list`
- 当前活动 torrent 中 **没有 literal IP announce**

新增域名：

- `zmpt.cc`
- `tracker.pterclub.com`
- `t.audiences.me`
- `t.hdhome.org`
- `tracker.hhanclub.net`
- `tracker.m-team.cc`
- `www.oshen.win`
- `tracker.agsvpt.cn`
- `t.pthome.org`
- `www.dragonhd.xyz`
- `tracker.hdsky.me`
- `tracker.totheglory.im`
- `tracker.keepfrds.com`
- `hdfans.org`
- `pt.soulvoice.club`

备注：

- `rules/custom/direct/trackers-ip.list` 继续保留为 literal IP 专用位
- 以后如果 qB/TR 真出现 `udp://1.2.3.4:6969/announce` 这种，再补到 IP 列表

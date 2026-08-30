# Wayne Shadowrocket China Config

适用于中国大陆网络环境的 Shadowrocket 自用分流配置。

## 目标

- 🇨🇳 中国大陆网站 / App 优先 `DIRECT`
- ✈️ Telegram 强制 `PROXY`
- 💬 WhatsApp 强制 `PROXY`
- 📞 Apple FaceTime / iMessage 优先 `DIRECT`
- 🎥 腾讯会议优先 `DIRECT`
- 🤖 ChatGPT / OpenAI 强制 `PROXY`
- 🌍 Google / YouTube / X / Instagram / Facebook / GitHub 等海外服务走 `PROXY`
- 🍎 Apple / iCloud / APNs 优先 `DIRECT`
- 未识别的中国 IP → `DIRECT`
- 未识别的其他流量 → `PROXY`

## 主配置

`Wayne-China-TG-WA-Push.conf`

当前版本：**V1.1**

### V1.1 变化

- 新增 Apple FaceTime / iMessage 显式直连
- 新增腾讯会议核心域名显式直连
- 保留 Telegram / WhatsApp 的独立代理规则
- 保留 APNs / Apple 系统服务直连策略

## 网络导入

推荐使用：

`https://cdn.jsdelivr.net/gh/OpenedAPI/shadowrocket-config@main/Wayne-China-TG-WA-Push.conf`

Shadowrocket：

`配置 → + → 下载配置 → 粘贴 URL`

导入后选中 `Wayne-China-TG-WA-Push.conf`，首页设置：

`全局路由 → 配置`

## 推荐设置

| 设置 | 状态 |
|---|---|
| 全局路由 | 配置 |
| 强制路由 | 关闭 |
| 包括所有网络 | 开启 |
| 包括本地网络 | 关闭 |
| 包括 APNs | 关闭 |
| 包括蜂窝服务 | 关闭 |
| APNs Push Fix 模块 | 不启用 |

## 分流逻辑

| 服务 | 路由 |
|---|---|
| 微信 / 淘宝 / 高德 / 国内 App | DIRECT |
| 腾讯会议 | DIRECT |
| FaceTime / iMessage | DIRECT |
| Apple / iCloud / APNs | DIRECT |
| Telegram | PROXY |
| WhatsApp | PROXY |
| ChatGPT / OpenAI | PROXY |
| Google / YouTube | PROXY |
| X / Instagram / Facebook | PROXY |
| GitHub | PROXY |
| 未识别中国 IP | DIRECT |
| 未识别其他流量 | PROXY |

## 建议测试

在 Shadowrocket 的“测试规则”中检查：

| 域名 | 预期 |
|---|---|
| `telegram.org` | PROXY |
| `t.me` | PROXY |
| `whatsapp.com` | PROXY |
| `chatgpt.com` | PROXY |
| `google.com` | PROXY |
| `github.com` | PROXY |
| `facetime.apple.com` | DIRECT |
| `meeting.tencent.com` | DIRECT |
| `wemeet.tencent.com` | DIRECT |
| `baidu.com` | DIRECT |
| `weixin.qq.com` | DIRECT |

## Telegram / WhatsApp Push

本配置将 Telegram / WhatsApp 的业务连接强制走代理，但不建议把 Apple APNs 强制塞进海外代理。

推荐结构：

`Telegram / WhatsApp 业务流量 → Shadowrocket → PROXY`

`Apple APNs → DIRECT`

这样更符合 iOS 的系统 Push 工作方式，也减少 APNs 因绕路产生额外延迟的可能性。

## FaceTime / 腾讯会议

FaceTime 和腾讯会议属于实时音视频业务，对延迟、抖动和 UDP 质量敏感。

因此在中国大陆使用时，本配置把它们放在通用海外代理规则之前明确 `DIRECT`，避免不必要地绕海外代理节点。

## 规则来源

主要依赖：

- `blackmatrix7/ios_rule_script`
- Telegram rules
- WhatsApp rules
- OpenAI rules
- Apple rules
- China rules
- Proxy rules

主配置尽量保持精简，服务域名/IP变化主要由远程规则维护。

## 说明

- 不启用 MITM
- 不安装 CA 证书
- 不集成广告过滤
- 不进行 HTTPS Rewrite

不同运营商、Wi-Fi、代理节点、iOS 与 Shadowrocket 版本可能存在差异。出现问题时优先确认实际规则命中情况。

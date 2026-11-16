# loon-rules — Loon 动静分离基础底座与自用分流规则库

采用 **「节点/证书留本地，自定义规则远程热更新」** 的动静分离架构：
- **静态本地层**：手机 / Mac 导入一次 `Loon.conf` 后，在设备本地填入机场订阅、自建/住宅代理链并信任 MitM 证书，后续**无需重复导入覆盖配置文件**，彻底避免节点被冲掉或重装证书。
- **动态远程层**：日常需要新增或调整分流域名时，直接修改 `rules/` 目录下的 `.list` 文件并推送至 GitHub，Loon 会在后台自动更新（或在 `配置 -> 外部资源` 手动下拉刷新立即生效）。

## 规则文件与策略映射（位于 `[Remote Rule]` 最顶部，优先级高于公共规则集）

| 文件路径 | 绑定策略组 | 用途说明 |
| :--- | :--- | :--- |
| `rules/Reject.list` | `REJECT` | 自用去广告 / 追踪 / 烦人域名屏蔽补充 |
| `rules/Direct.list` | `DIRECT` | 自用强制直连补充（被误走代理的国内站点、网银、国内大体积更新 CDN） |
| `rules/AI.list` | `AI专线` | 自用 AI 工具、模型 API、AI IDE 域名补充 |
| `rules/Payment.list` | `跨境支付` | 跨境支付结账与 IP 纯净度 / DNS 防泄露检测站集合 |
| `rules/Proxy.list` | `默认代理` | 日常遇到打不开或被阻断的小众海外网站补充 |

## 编写语法提示

`.list` 规则文件内**每行只写匹配类型与域名/IP，末尾不要加策略名**（策略已在 `Loon.conf` 的 `[Remote Rule]` 中统一绑定）：

```ini
DOMAIN,api.example.com
DOMAIN-SUFFIX,example.com
DOMAIN-KEYWORD,example
IP-CIDR,1.2.3.0/24,no-resolve
```

> **安全提示**：本仓库不含任何代理节点、订阅链接或证书私钥。若需在本地备份已填入节点与 `ca-p12` 私钥的配置，请命名为 `*.local.conf` 或 `*.private.conf`（已被 `.gitignore` 自动拦截，防止误提交）。

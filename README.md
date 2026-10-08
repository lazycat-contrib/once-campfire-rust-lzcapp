# once-campfire-rust-lzcapp

[ONCE Campfire](https://github.com/basecamp/once-campfire-rust) 的懒猫微服打包（Rust 移植版）。

- 上游镜像：`ghcr.io/basecamp/once-campfire-rust`（镜像模式，经 `ghcr.1ms.run` 拉取）
- 数据持久化在 `/lzcapp/var/storage`（SQLite 数据库、上传文件、备份）
- TLS 由外层终止（懒猫网关或自定义反代/CDN），容器内以 `DISABLE_SSL=1` 走明文 HTTP
- Web Push 使用内置默认 VAPID 密钥对（可在安装向导中替换；全部留空则推送关闭）

## 设置向导参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `custom_domain` | 空 | 自定义对外域名（不带 `https://` / `http://`；用于推送通知链接；留空使用懒猫域名） |
| `vapid_public_key` | 内置默认 | Web Push 公钥（P-256，URL-safe Base64） |
| `vapid_private_key` | 内置默认 | Web Push 私钥（与公钥配对） |

运行时 `VAPID_SUBJECT` 的取值：

1. **未填 `custom_domain`**：`https://<懒猫域名>`（默认）。
2. **填写 `custom_domain`**：`https://<custom_domain>`——请自行将域名指向本应用并在外部终止 TLS（如 Cloudflare 或自有反代）。

> 容器不设置 `TLS_DOMAIN`：在该上游版本中它会启用容器内置 ACME 并让 80 端口强制 301 到 HTTPS、只认单一 Host（其余返回 421），在懒猫网关或 Cloudflare 代理后会造成重定向环。若确需容器直接申请 Let's Encrypt 证书（要求域名公网可达），可手动改用 `TLS_DOMAIN=<域名>` 并移除 `DISABLE_SSL`。
>
> 注：Rust 移植版不发送遥测，不支持 `SENTRY_DSN`；Sentry 参数未提供。

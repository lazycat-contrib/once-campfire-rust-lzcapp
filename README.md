# once-campfire-rust-lzcapp

[ONCE Campfire](https://github.com/basecamp/once-campfire-rust) 的懒猫微服打包（Rust 移植版）。

- 上游镜像：`ghcr.io/basecamp/once-campfire-rust`（镜像模式，经 `ghcr.1ms.run` 拉取）
- 数据持久化在 `/lzcapp/var/storage`（SQLite 数据库、上传文件、备份）
- 网关负责 TLS，容器内以 `DISABLE_SSL=1` 走明文 HTTP
- Web Push 使用内置默认 VAPID 密钥对（可在安装向导中替换；全部留空则推送关闭）

## 设置向导参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `enable_custom_domain` | `false` | 启用自定义域名（内置 Let's Encrypt TLS） |
| `custom_domain` | 空 | 自定义域名（不带 `https://` / `http://`；需公网可达以申请证书） |
| `vapid_public_key` | 内置默认 | Web Push 公钥（P-256，URL-safe Base64） |
| `vapid_private_key` | 内置默认 | Web Push 私钥（与公钥配对） |

两种运行模式：

1. **默认（懒猫域名）**：`DISABLE_SSL=1`，网关终止 TLS；`VAPID_SUBJECT` 指向懒猫域名，推送通知链接正确。
2. **自定义域名**：`TLS_DOMAIN=<域名>` 触发 front 内置 ACME（Let's Encrypt），Rails 保持 `force_ssl` 语义。

> 注：Rust 移植版不发送遥测，不支持 `SENTRY_DSN`；Sentry 参数未提供。

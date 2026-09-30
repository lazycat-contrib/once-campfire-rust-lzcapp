# once-campfire-rust-lzcapp

[ONCE Campfire](https://github.com/basecamp/once-campfire-rust) 的懒猫微服打包（Rust 移植版）。

- 上游镜像：`ghcr.io/basecamp/once-campfire-rust`（镜像模式，经 `ghcr.1ms.run` 拉取）
- 数据持久化在 `/lzcapp/var/storage`（SQLite 数据库、上传文件、备份）
- 网关负责 TLS，容器内以 `DISABLE_SSL=1` 走明文 HTTP
- Web Push 需要可选的 VAPID 密钥对（安装时在设置向导填写；不填则推送关闭）

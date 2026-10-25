# AGENTS.md

## 仓库概述

Docker 镜像构建仓库（GitHub: `tossp/compiler`），通过 GitHub Actions 构建并发布自定义 Docker 镜像和二进制。无应用代码、无测试、无 lint。

## 目录结构

- `compose/<name>/dockerfile` — 各镜像的 Dockerfile
- `.github/workflows/` — CI 构建流水线
- 当前镜像：`hs-derp`（Tailscale DERP server）、`jellyfin`（中文字体定制版）

## CI 工作流

| 工作流 | 触发方式 | 作用 |
|---|---|---|
| `caddy.yml` | 手动 / 每月1日 / push workflows | xcaddy 多平台编译 + UPX 压缩，发布到 GitHub Release (`auto-release`) |
| `composer.yml` | 手动 / 每周五 / push 自身 | 多平台 Docker 构建，推送 DockerHub (`tossp/*`) + GHCR |
| `mirror.yml` | 手动 / push 自身 | 拉取第三方镜像并 retag 到私有 registry |

## 关键约定

- Dockerfile 命名为小写 `dockerfile`（非 `Dockerfile`）
- 镜像 tag 格式：`latest` + `YYYYMMDD` 日期标签
- `composer.yml` 需要 secrets：`DOCKERHUB_USERNAME`、`DOCKERHUB_TOKEN`、`GITHUB_TOKEN`
- `caddy.yml` 通过 env `PLUGINS` 管理 Caddy 插件列表，修改插件只需编辑该变量
- `hs-derp` 支持多架构（amd64/arm64/armv6/v7），`jellyfin` 仅 `linux/amd64`

## 修改指南

- 新增镜像：在 `compose/` 下建目录 + `dockerfile`，在 `composer.yml` 中添加构建步骤
- 修改 Caddy 插件：编辑 `caddy.yml` 中 `env.PLUGINS`
- workflow 文件修改会触发自身重建（通过 `paths` 过滤）

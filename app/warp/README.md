# warp

Warp Windows 构建验证上下文目录。

## 目的

- 使用 GitHub Actions 的 Windows runner 验证 `warpdotdev/warp` 开源仓库能否完成最小 OSS 客户端构建
- 避免在本地 Windows 主机安装和污染构建依赖
- 为后续长期维护 Warp 构建流程预留说明、脚本、补丁和记录位置

## 当前约定

- 执行入口：`.github/workflows/warp.yml`
- 当前目标：仅做最小 Windows 构建验证
- 不包含：installer 打包、签名、正式发布、Docker 镜像构建

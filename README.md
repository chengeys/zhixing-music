# 知行音乐 🎵

NAS 私有曲库网页播放器。在线地址：https://chengeys.github.io/nas-music-player/

## 简介

基于 WebDAV 的 NAS 音乐播放器，支持：
- 3633 首曲库浏览、搜索
- 流式播放 + 后台整首预取
- 锁屏/耳机控制（Media Session）
- 歌词滚动、专辑封面
- PWA 可添加到主屏幕
- 车载模式

## 技术栈

纯前端三件套，无构建：`index.html` + `style.css` + `app.js`
- `catalog.js`：曲库索引（3633 首，678KB）
- `sw.js`：Service Worker，注入 NAS 认证头
- `manifest.json`：PWA 配置

## 部署

推送到 `chengeys/nas-music-player` 仓库即自动部署到 GitHub Pages。

## 开发记录

- `CHANGELOG.md`：版本更新记录
- `ISSUES.md`：已知问题与排查记录（锁屏连播等）

# NAS 连接方式

> ⚠️ 密码不记录在此。NAS 账号密码由用户手机本地保存，一次性使用后丢弃。

## 基本信息

- **设备**：群晖 Synology NAS
- **系统**：DSM 7.4.1-90080
- **账号**：`ai`（非管理员，最小权限）
- **共享文件夹**：`ai`、`docker`、`home`、`video`、`宝塔备份ftp`
- **配额**：`ai` 在 volume2，配额 10GB（已用约 2.3GB）⚠️ 备份每天约 800MB，约 12 天写满

## WebDAV（音乐播放器用）

播放器通过 WebDAV 直链播放 NAS 上的音乐文件。

### 外网地址（浏览器/手机用）

```
https://nas.yjm.ccwu.cc/dav
```

- 用户配的 443 反代，浏览器可直接打开 DSM 登录页
- 播放器 `songUrl()` 生成的就是这个地址 + 文件路径
- 认证：HTTP Basic Auth（用户名 `ai` + 密码）
- Service Worker 在 `fetch` 时注入 `Authorization` 头（见 `sw.js` 的 `pushAuthToSW` / `getAuth`）
- 密码由用户在设置页输入，存 `localStorage`（注意：无痕模式下不持久）

### 内网地址（服务器脚本用）

```
http://nas.hbjdf.cn:9001
```

- 本机 VM 2 秒直达
- 注意：`198.18.0.0/15` 是保留网段，浏览器出口网关拦截，走不通；脚本走这个地址
- 三站备份脚本 `~/workspace/nas-backup/webbackup.sh` 用此地址推送

### 音乐库路径

```
/music/                    # 3028 首（3009 MP3 + 19 FLAC）
/music/车载/               # 98% 在此（DJ/榜单/发烧合集）
/music/知行/               # 知行建的华语新歌单
```

### 特殊路径注意

- R&B 目录真名是 `R&B`，WebDAV XML 会把 `&` 转义成 `&amp;`，代码里要处理
- 排除（不删文件，只不收录）：
  - `/music/jxs/` 4 个非歌曲录音
  - `/music/kejian/` 22 个课程录音

## 三站备份（WebDAV 推送）

- 脚本：`~/workspace/nas-backup/webbackup.sh`
- 目标：`ai` 共享文件夹（经 WebDAV 5006 端口）
- 频率：每天 02:30（宝塔计划任务）
- 保留：30 天
- 内容：三站网站目录打包（含 SQLite，无独立 MySQL）

## DSM 网页管理

- 地址：`https://nas.yjm.ccwu.cc`（反代，平时用户关闭，需要时他打开）
- `ai` 账号登录后仅见 File Station / Drive / Photos（非管理员）

## 注意事项

1. **不要索要管理员权限**：用户明确拒绝，只给 `ai` 最小权限。
2. **不要删除 NAS 文件**：除非用户明确授权。
3. **配额告警**：`ai` 仅 10GB，备份 12 天写满，需用户调大配额或缩短保留期。
4. **反代开关**：`nas.yjm.ccwu.cc` 的反代平时关闭，需要时用户手动开。

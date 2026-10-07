# 更新日志

## v9.3 (2026-10-07)
- 修锁屏连播：playing 事件里重建 media session（iOS 会清空播放开始前注册的 handlers，致按钮变灰）；iOS 显式置空 seekbackward/seekforward/seekto，还原上一首/下一首按钮；playbackState 同步真实状态
- 锁屏 play() 被系统拒绝后自动恢复：解锁/点按一次自动重试播放；preSwitch 失败也进重试
- 加 navigator.audioSession.type='playback'；启动时清 WebKit 陈旧 media session 状态

## v9.2 (2026-10-07)
- preSwitch 回滚到 v8.5 版本（v9.1 的回滚逻辑反而导致卡死）
- updateMediaSession 简化到最简（只设4个处理器）

## v9.1 (2026-10-07)
- 修 preSwitch 失败时回滚 qi（但实际导致更严重的卡死，已在 v9.2 回滚）

## v9.0 (2026-10-07)
- 明确清除 seekbackward/seekforward/seekto，只留 prev/next
- ended 加调试 toast（后又去掉）

## v8.8 (2026-10-07)
- 去掉 setPositionState（no-op），去掉 seekto，还原 v8.5 逻辑
- 但引入 preSwitch 状态 bug（v9.1 试图修，v9.2 回滚）

## v8.7 (2026-10-07)
- setPositionState 加保护（过渡期不上报、接近结尾不上报）
- 加 posStateBlocked 标志

## v8.6 (2026-10-07)
- 恢复 setPositionState 到 timeupdate（1秒节流）→ 导致连播中断、按钮变快进
- iOS 语音改走键盘听写

## v8.5 (2026-10-07) ✅ 锁屏连播曾可用
- audio.autoplay=true
- 语音 aborted 自动重试一次

## v8.4 (2026-10-07)
- 语音报错显示具体错误码
- preSwitch 加直链兜底（blob 没下好也试）

## v8.3 (2026-10-07)
- 设置页加强制更新按钮
- SW 不再预缓存 catalog.js（678KB 大文件导致安装失败）

## v8.2 (2026-10-07)
- preSwitch：结束前2.5秒提前切歌（iOS 当连续播放放行）

## v8.1 (2026-10-07)
- 播放逻辑：play() 提前到 UI 更新之前
- 预取下一首（音频+歌词+封面）

## v8.0 (2026-10-07)
- QQ音乐风重设计（绿色主题）
- 去掉 setPositionState（省电）→ 导致锁屏进度条不可拖

## v7.x 及更早
- 流式播放、Service Worker 认证注入
- 502/503/504 自动重试
- 推荐系统、播放记录、收藏

# 已知问题与排查记录

## 🔴 锁屏自动连播（iOS）

**现状（2026-10-07 v9.2）**：
- 不锁屏：自动切下一首 ✅ 正常
- 锁屏：播完不自动进下一首 ❌
- 锁屏按钮：上一首/下一首显示为灰色（disabled）❌

**曾可用**：v8.5 时用户确认锁屏连播可用。

**已尝试的方案**：
1. **ended 事件直接 play()**：iOS 锁屏时拦截无手势的 play()，失败。
2. **preSwitch（提前2.5秒切歌）**：v8.2 引入。理论上在还在播放时切歌，iOS 当连续播放放行。v8.5 时用户说可用，但后续版本失效。
3. **audio.autoplay=true**：v8.5 引入。换 src 后浏览器自动播。
4. **setPositionState 上报位置**：v8.6 加回导致连播中断、按钮变快进10秒；v8.8 去掉。
5. **preSwitch 失败回滚**：v9.1 尝试，实际导致更严重的卡死；v9.2 回滚。

**preSwitch 状态 bug（已确认）**：
```js
// preSwitch 里先改 qi 和 src，再调 play()
qi=ni; 
audio.src=...;
audio.play().catch(()=>{
  song._preSwitched=false; 
  // ❌ 没回滚 qi！导致 ended 事件用错的 qi，状态错乱
});
```
v9.1 试图回滚 qi 并调 next()，但 next() 里的 play() 在锁屏也被拦，导致卡死。v9.2 回滚到 v8.5 版本。

**按钮灰色问题**：
- iOS 根据 setActionHandler 决定按钮是否可用。
- v8.6/v8.7 设置了 seekto，导致显示快进/快退按钮。
- v9.0/v9.1 试图清除 seek 处理器，但按钮变灰（disabled）。
- 怀疑 iOS 在系统层缓存了旧的媒体会话状态，需彻底杀掉 Safari 进程才能重置。

**待验证**：
- 杀掉 Safari 进程重开后，v9.2 按钮是否恢复可用。
- v8.5 到底用了什么魔法让锁屏连播可用（需对比代码差异）。

---

## 🟡 iOS 网页语音识别

**现状**：`webkitSpeechRecognition` 频繁报 `aborted`，重试也失败。

**结论**：iOS 网页语音识别是系统层问题，修不好。

**方案**（v8.6 起）：iOS 上点麦克风直接聚焦输入框，提示用户点键盘 🎤 用系统听写（原生，更准）。

---

## 🟢 已解决

- Service Worker 缓存导致无法更新 → 设置页加强制更新按钮（v8.3）
- 播放时 AbortError → playing/waiting/pause 状态保护
- R&B 路径 `&amp;` 编码问题 → 修正139条路径
- 502/503/504 → 自动重试3次

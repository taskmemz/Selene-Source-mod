# Selene

A MoonTV-based cross-platform video player built with Flutter.

基于 Flutter 的跨平台视频播放器。

## Features / 功能

- Multi-source video search & playback / 多源视频搜索与播放
- Live TV with M3U/EPG support / 直播电视（M3U/EPG 支持）
- Douban movie/TV/anime recommendations / 豆瓣影视推荐
- Server mode & local mode support / 服务端模式与本地模式
- DLNA casting / DLNA 投屏
- Cross-platform / 跨平台：Windows / macOS / Linux / Android / iOS / Web

---

## Changelog / 更新日志

### 1.6.7

#### Fix: WAF reverse proxy cookie conflict / 修复 WAF 反代 Cookie 冲突

- **Problem**: When behind SafeLine WAF (雷池), the `Set-Cookie` header contains multiple cookies joined by `,`. The old `_parseCookies` used `split(';')[0]` which only captured `sl-waiting-session`, discarding the actual `auth` session cookie. Login succeeded but subsequent API calls returned 401, redirecting back to the login screen.
- **Fix**: Rewrote `_parseCookies` to split by `\n` and `,`, extract every `name=value` before `;`, and filter out invalid fragments via `contains('=')`.
- **问题**：使用雷池 WAF 时，`Set-Cookie` 包含多个以 `,` 拼接的 Cookie。原 `_parseCookies` 用 `split(';')[0]` 只取到了 `sl-waiting-session`，丢失了真正的 `auth` 会话 Cookie。导致登录成功但后续 API 请求返回 401，被踢回登录页。
- **修复**：重写 `_parseCookies`，按 `\n` 和 `,` 切分、提取每个 `;` 之前的 `name=value`，并用 `contains('=')` 过滤掉无效碎片。

#### Feat: Advanced settings on login screen / 登录页高级设置

- Added a collapsible "高级设置" panel with Custom User-Agent, browser headers toggle (Sec-Fetch-*, Sec-CH-UA-*), and custom header name/value for WAF whitelist bypass.
- All API requests (`ApiService._buildHeaders`) and auto-login use these headers.
- 新增可折叠的"高级设置"面板：自定义 User-Agent、浏览器特征头开关（Sec-Fetch-*、Sec-CH-UA-*）、自定义请求头（用于 WAF 白名单绕过）。所有 API 请求和自动登录均使用这些设置。

#### Fix: media_kit version conflict / 修复 media_kit 版本冲突

- `media_kit: ^1.2.6` was incompatible with `media_kit_video: ^2.0.1`. Downgraded `media_kit_video` to `^1.3.1`.
- `media_kit: ^1.2.6` 与 `media_kit_video: ^2.0.1` 不兼容（2.x 需要 media_kit >=2.0.0）。`media_kit_video` 降级至 `^1.3.1`。

#### Fix: Windows title bar hardcoded black / 修复 Windows 标题栏硬编码纯黑

- Player screens used `Color(0xFF000000)` for the custom title bar. Changed to theme-aware colors.
- 播放器页面标题栏使用 `Color(0xFF000000)` 纯黑，改为跟随主题（暗色 `#121212`，亮色 `#F5F5F5`）。

#### Chore: Player initialization / Player 初始化

- Added explicit `PlayerConfiguration` with `title` and `bufferSize` to `media_kit Player()`.
- `media_kit Player()` 构造函数传入显式 `PlayerConfiguration`。

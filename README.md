# Selene

<div align="center">
  <img src="logo.png" alt="Selene Logo" width="120">
</div>

> 🎬 **Selene** 是以 [MoonTV](https://github.com/MoonTechLab/LunaTV) v100 版本 / [Helios](https://github.com/MoonTechLab/Helios) 为后端的客户端，保证原汁原味的同时，优化了移动端和桌面端操作体验。它基于 **Flutter** 构建，目前支持 Android、iOS、macOS 和 Windows 平台。

<div align="center">

![Flutter](https://img.shields.io/badge/Flutter-3.4.3+-02569B?logo=flutter)
![Dart](https://img.shields.io/badge/Dart-3.4.3+-0175C2?logo=dart)
![Android](https://img.shields.io/badge/Android-5.0+(API_21)-3DDC84?logo=android)
![iOS](https://img.shields.io/badge/iOS-13.0+-000000?logo=ios)
![macOS](https://img.shields.io/badge/macOS-11.0+-000000?logo=apple)
![Windows](https://img.shields.io/badge/Windows-10+-0078D6?logo=windows)

</div>

<details>
  <summary>点击查看移动端截图</summary>
  <img src="screenshot/Screenshot_1.png" alt="项目截图" width=300>
  <img src="screenshot/Screenshot_2.png" alt="项目截图" width=300>
  <img src="screenshot/Screenshot_3.png" alt="项目截图" width=300>
  <img src="screenshot/Screenshot_4.png" alt="项目截图" width=300>
  <img src="screenshot/Screenshot_5.png" alt="项目截图" width=300>
  <img src="screenshot/Screenshot_6.png" alt="项目截图" width=300>
  <img src="screenshot/Screenshot_8.png" alt="项目截图" width=300>
  <img src="screenshot/Screenshot_7.png" alt="项目截图">
</details>

<details>
  <summary>点击查看 pc 端 / 宽屏设备截图</summary>
  <img src="screenshot/Screenshot_9.png" alt="项目截图">
  <img src="screenshot/Screenshot_10.png" alt="项目截图">
  <img src="screenshot/Screenshot_11.png" alt="项目截图">
  <img src="screenshot/Screenshot_12.png" alt="项目截图">
  <img src="screenshot/Screenshot_13.png" alt="项目截图">
  <img src="screenshot/Screenshot_14.png" alt="项目截图">
</details>

### 请不要在 B站、小红书、微信公众号、抖音、今日头条或其他中国大陆社交平台发布视频或文章宣传本项目，不授权任何"科技周刊/月刊"类项目或站点收录本项目。

---

## ✨ 功能特性

### 🎯 核心功能
- **多源聚合搜索** - 支持多个视频源的聚合搜索，快速找到想看的内容
- **智能播放记录** - 自动记录播放进度，支持断点续播
- **个人收藏夹** - 收藏喜欢的影视作品，方便随时观看
- **多平台支持** - 支持电影、电视剧、动漫、综艺等多种内容类型
- **DLNA 投屏** - 大屏看片就是爽

### 🎨 用户体验
- **现代化 UI** - 基于 Material Design 3 的现代化界面设计
- **深色模式** - 支持深色/浅色主题切换，护眼更舒适
- **流畅动画** - 丰富的交互动画，提升使用体验

### 🔧 技术特性
- **高性能播放** - 移动端使用 Awesome Video Player + FVP 后端，桌面端使用 Media Kit，支持多种视频格式
- **智能缓存** - 图片缓存和数据缓存机制，提升加载速度
- **网络优化** - 支持 WebSocket 实时通信，响应更迅速
- **跨平台适配** - 针对不同平台优化的播放器和 UI 控件

## 📱 支持平台

- **Android** - 最低支持 Android 5.0 (API 21)
- **iOS** - 最低支持 iOS 13.0
- **macOS** - 最低支持 macOS 11.0 (Big Sur)
- **Windows** - 最低支持 Windows 10

## 📖 使用说明

### 首次使用
1. 启动应用后，系统会自动检查登录状态
2. 如未登录，会跳转到登录页面
3. 登录成功后进入主界面

### 主要功能
- **首页** - 查看热门内容、继续观看、个人收藏
- **搜索** - 多源聚合搜索，支持实时搜索建议
- **分类浏览** - 按电影、电视剧、动漫、综艺分类浏览
- **播放器** - 支持多种播放控制，自动记录播放进度

## 🏗️ 技术架构

### 核心技术栈
- **Flutter 3.4.3** - 跨平台 UI 框架
- **Dart 3.4.3** - 编程语言
- **Provider** - 状态管理
- **Dio** - HTTP 网络请求
- **FVP** - 播放器后端
- **Awesome Video Player** - 移动端播放器前端
- **Media Kit** - 桌面端播放器前端
- **Cached Network Image** - 图片缓存
- **DLNA Dart** - 投屏功能

## 📋 更新日志

### 1.6.7

#### 🐛 修复：WAF 反代 Cookie 冲突导致登录后被踢回

- **问题**：使用雷池 (SafeLine)等 WAF之类 反向代理时，`Set-Cookie` 响应头包含多个以 `,` 拼接的 Cookie（`sl-waiting-session`、`sl-waiting-state`、`auth`、`sl-session`）。原来的 `_parseCookies` 用 `split(';')[0]` 只取到了第一个 `sl-waiting-session`，丢失了真正的业务 `auth` 会话 Cookie。导致登录请求返回 200，但后续 API 请求因缺少 auth Cookie 返回 401，立即被踢回登录页。
- **修复**：重写 `_parseCookies`，按 `\n` 和 `,` 切分多个 Cookie，提取每个 `;` 之前的 `name=value`，并用 `contains('=')` 过滤掉 Expires 日期等无效碎片，确保 WAF 注入的 Cookie 和业务 Cookie 全部正确保留。

#### ✨ 新增：登录页高级设置面板

- 登录表单下方新增可折叠的"高级设置"面板
- **自定义 User-Agent**：默认 Chrome 124 浏览器 UA，可手动修改
- **浏览器特征头开关**：开启后自动注入 `Accept-Language`、`Sec-Fetch-*`、`Sec-CH-UA-*` 等头字段
- **自定义请求头**：可设置任意名称/值的请求头（如 `X-Bypass: selene`），配合 WAF 白名单规则绕过 Bot 检测
- 所有后续 API 请求（播放记录、收藏、搜索等）及自动登录均使用以上自定义头

#### 🔧 修复：media_kit 版本冲突

- `media_kit: ^1.2.6` 与 `media_kit_video: ^2.0.1` 版本不兼容（v2.x 依赖 media_kit >=2.0.0）
- 将 `media_kit_video` 降级为 `^1.3.1`，统一使用 1.x 系列

#### 🎨 修复：Windows 标题栏硬编码纯黑

- 视频播放页和直播播放页的 `WindowsTitleBar` 使用 `Color(0xFF000000)` 纯黑背景
- 改为跟随主题：暗色模式 `#121212`，亮色模式 `#F5F5F5`，与首页一致

#### 🔨 优化：Player 初始化配置

- `media_kit Player()` 构造函数传入显式 `PlayerConfiguration`，设置 title 和 buffer 参数

## ⚠️ 免责声明

**重要提醒：**

1. **仅供学习交流** - 本项目仅用于技术学习和交流目的，不提供任何商业服务。

2. **内容来源** - 本应用聚合的内容来源于第三方平台，我们不对内容的合法性、准确性、完整性或可用性承担任何责任。

3. **版权声明** - 所有影视内容的版权归原作者和版权方所有，请用户自觉遵守相关法律法规，支持正版。

4. **使用风险** - 用户使用本应用所产生的任何直接或间接损失，开发者不承担任何责任。

5. **合规使用** - 请用户在使用过程中遵守当地法律法规，不得用于任何违法用途。

6. **数据安全** - 虽然我们重视用户隐私，但请用户自行承担数据安全风险。

**使用本应用即表示您已阅读并同意上述免责声明。**

## 🙏 致谢

- [MoonTV](https://github.com/MoonTechLab/LunaTV) - 后端服务支持
- [Flutter](https://flutter.dev/) - 跨平台开发框架
- 所有用户的支持
---

<div align="center">
  <p>如果这个项目对您有帮助，请给个 ⭐️ 支持一下！</p>
</div>

[![Star History Chart](https://api.star-history.com/svg?repos=MoonTechLab/Selene&type=Date)](https://www.star-history.com/#MoonTechLab/Selene&Date)

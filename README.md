# 通话生成 - Android 通话记录批量生成工具

一款高仿系统级 Android 通话记录批量生成应用，支持将生成的通话记录写入系统拨号记录。

---

## 功能特性

| 功能 | 说明 |
|------|------|
| 单个生成 | 手动填写号码、时间、时长，生成一条通话记录 |
| 批量生成 | 在时间范围内按随机间隔自动生成多条记录 |
| 归属地随机 | 自动随机生成中国大陆手机号 |
| 指定号码 | 每行填入一个号码，循环批量生成 |
| 通话类型 | 呼出 / 呼入 / 未接 / 拒接 / 随机 |
| 写入系统 | 通过 ContentProvider 写入 Android 系统通话记录 |
| 模板保存 | 保存常用参数配置为模板 |
| 深色模式 | 支持跟随系统 / 手动切换 |

---

## 项目结构

```
myapp/
├── app/
│   ├── _layout.tsx              # 根路由布局
│   └── (tabs)/
│       ├── _layout.tsx          # Tab 导航（含全局主题 Context）
│       ├── index.tsx            # 生成页
│       ├── records.tsx          # 记录页
│       └── about.tsx            # 关于页
├── src/
│   ├── constants/index.ts       # 常量（颜色、通话类型等）
│   ├── types/index.ts           # TypeScript 类型定义
│   ├── utils/index.ts           # 工具函数（生成算法、格式化等）
│   ├── hooks/index.ts           # 自定义 Hooks（存储、主题）
│   └── components/ui.tsx        # 通用 UI 组件库
├── modules/
│   └── call-log-writer/
│       ├── android/
│       │   ├── build.gradle
│       │   └── src/main/java/expo/modules/calllogwriter/
│       │       └── CallLogWriterModule.kt   # Android 原生写入模块
│       ├── src/index.ts         # JS 侧原生模块封装
│       └── package.json
├── app.json                     # Expo 配置（含 Android 权限）
└── eas.json                     # EAS Build 构建配置
```

---

## 所需 Android 权限

```xml
android.permission.READ_CALL_LOG
android.permission.WRITE_CALL_LOG
android.permission.READ_PHONE_STATE
android.permission.PROCESS_OUTGOING_CALLS
```

安装 APK 后，首次写入时请在系统设置中授予**通话记录**权限。

---

## 构建 APK（推荐使用 EAS Build）

### 前提

```bash
npm install -g eas-cli
eas login        # 登录 Expo 账号（免费）
```

### 构建预览版 APK

```bash
cd myapp
eas build --platform android --profile preview
```

构建完成后下载 `.apk` 文件安装到 Android 设备即可。

### 本地构建（需要配置 Android SDK）

```bash
cd myapp
npx expo prebuild --platform android
cd android && ./gradlew assembleRelease
# APK 位于 android/app/build/outputs/apk/release/app-release.apk
```

---

## 开发调试

```bash
cd myapp
pnpm install
npx expo start --android   # 需要连接 Android 设备或模拟器
```

> ⚠️ **注意**：写入系统通话记录功能仅在通过 EAS Build 或本地构建的 APK 中可用。  
> Expo Go 不支持自定义原生模块，无法使用写入功能（生成和预览功能正常）。

---

## 兼容性说明

- 最低 Android 版本：7.0 (API 24)
- 目标 Android 版本：14 (API 34)
- 部分厂商（MIUI、ColorOS、HarmonyOS）可能需要额外的"后台弹出"或"修改通话记录"权限
- SIM 卡槽指定功能在部分设备上可能不生效（系统限制）

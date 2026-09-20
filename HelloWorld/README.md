# HelloWorld（Android）

一个最小可运行的 Android 原生工程：Kotlin + XML 布局，打开显示一行 **Hello World!**。

## 技术栈

| 组件 | 版本 |
|---|---|
| Android Gradle Plugin (AGP) | 8.5.2 |
| Gradle | 8.7（Wrapper 已包含，直接用 `./gradlew`） |
| Kotlin | 1.9.24 |
| compileSdk / targetSdk | 34（Android 14） |
| minSdk | 24（Android 7.0） |
| JDK | 17（AGP 8.x 构建要求） |

## 工程结构

```
HelloWorld/
├── settings.gradle            # 仓库与模块声明
├── build.gradle               # 顶层：插件版本
├── gradle.properties          # Gradle / Android 配置
├── gradlew / gradlew.bat      # Gradle Wrapper（无需本地安装 Gradle）
├── gradle/wrapper/            # Wrapper 配置与 jar
└── app/
    ├── build.gradle           # 应用模块构建脚本
    └── src/main/
        ├── AndroidManifest.xml
        ├── java/com/example/helloworld/MainActivity.kt
        └── res/
            ├── layout/activity_main.xml   # 居中显示 Hello World!
            └── values/                    # strings / colors / themes
```

## 用 Android Studio 运行（推荐）

1. 打开 Android Studio → **Open** → 选择 `HelloWorld` 目录。
2. 首次打开会自动同步 Gradle（需联网下载依赖）。
3. 连接手机（开 USB 调试）或启动一个 API ≥ 24 的模拟器。
4. 点击 ▶ **Run 'app'**，屏幕中央显示 `Hello World!`。

## 用命令行构建

```bash
cd HelloWorld
./gradlew assembleDebug        # macOS / Linux
# gradlew.bat assembleDebug    # Windows
```

产物：`app/build/outputs/apk/debug/app-debug.apk`，可直接 `adb install` 到手机。

> 要求：JDK 17+，并设置好 `ANDROID_HOME`（或在工程根目录建 `local.properties` 写一行 `sdk.dir=你的SDK路径`）。

## GitHub Actions 自动构建

工程已内置 CI 工作流（`.github/workflows/android.yml`）：

1. 把工程推到 GitHub：
   ```bash
   git init
   git add .
   git commit -m "HelloWorld"
   git branch -M main
   git remote add origin <你的仓库地址>
   git push -u origin main
   ```
2. 每次 `push` / 提 PR 都会自动触发构建；也可以在仓库页 **Actions → Android CI → Run workflow** 手动触发。
3. 构建流程：配置 JDK 17 → 安装 Android SDK（platform-34、build-tools 34.0.0）→ `./gradlew assembleDebug` → 跑单元测试 → 上传 APK。
4. 构建成功后，在那次运行页面底部的 **Artifacts** 里下载 `app-debug`（内含 `app-debug.apk`，保留 14 天）。

> **排错**：如果 CI 报 `chmod: cannot access 'gradlew': No such file or directory`，
> 说明仓库里没有 `gradlew`，一般是 push 时目录层级不对（多包了一层文件夹）或漏传了文件。
> 检查仓库根目录是否直接包含 `gradlew`、`settings.gradle`、`app/`；新版工作流已能自动兼容
> `gradlew` 在根目录或一层子目录的情况，更新工作流文件后重推即可。

## 改包名 / 应用名

- 应用名：改 `app/src/main/res/values/strings.xml` 里的 `app_name`。
- 包名：改 `app/build.gradle` 的 `namespace` / `applicationId`，以及 `MainActivity.kt` 的 `package` 和目录。

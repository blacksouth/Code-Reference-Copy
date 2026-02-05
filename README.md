# Code-Reference-Copy
# Code Reference Copy - IntelliJ IDEA 插件

一个方便的 IntelliJ IDEA 插件，用于快速复制代码引用，生成带有文件路径和行号的引用格式。

## 功能特性

- ✅ 选中代码后，快速复制为带有文件路径和行号的引用格式
- ✅ 支持三种路径格式：包路径、相对路径、绝对路径
- ✅ 可在设置中自由切换路径格式
- ✅ 支持单行和多行代码引用
- ✅ 提供快捷键操作，提高效率

## 引用格式示例

### 包路径格式
```
com.jiuji.oa.stock.subpushlog.service.impl.SubPushLogServiceImpl.java#L160-171
```

### 相对路径格式
```
src/main/java/com/jiuji/oa/stock/subpushlog/service/impl/SubPushLogServiceImpl.java#L160-171
```

### 绝对路径格式
```
C:/project/src/main/java/com/jiuji/oa/stock/subpushlog/service/impl/SubPushLogServiceImpl.java#L160-171
```

## 使用方法

### 基本使用

1. 在编辑器中选中要引用的代码
2. 右键选择 **"Copy Code Reference"** 或使用快捷键 `Ctrl+Shift+Alt+C`
3. 粘贴即可得到格式化的代码引用

### 配置路径格式

1. 打开 **Settings/Preferences** (Windows/Linux: `Ctrl+Alt+S`, macOS: `Cmd+,`)
2. 导航到 **Tools > Code Reference Copy**
3. 选择您偏好的路径格式：
   - **包路径格式**：适合 Java 项目，显示包名结构
   - **相对路径格式**：显示相对于项目根目录的路径
   - **绝对路径格式**：显示文件的完整系统路径
4. 点击 **Apply** 或 **OK** 保存设置

## 安装方法

### 方式一：从源码构建

1. 克隆或下载本项目
2. 在项目根目录执行构建命令：
   ```bash
   ./gradlew buildPlugin
   ```
3. 构建完成后，在 `build/distributions/` 目录下会生成插件 zip 文件
4. 在 IntelliJ IDEA 中：
   - 打开 **Settings/Preferences > Plugins**
   - 点击齿轮图标 ⚙️ > **Install Plugin from Disk...**
   - 选择生成的 zip 文件
   - 重启 IDE

### 方式二：从 JetBrains Marketplace 安装（待发布）

1. 打开 **Settings/Preferences > Plugins**
2. 搜索 "Code Reference Copy"
3. 点击 **Install**
4. 重启 IDE

## 开发指南

### 环境要求

- JDK 17 或更高版本
- IntelliJ IDEA 2023.1 或更高版本
- Gradle 7.0 或更高版本

### 项目结构

```
code-reference-copy-plugin/
├── build.gradle.kts                    # Gradle 构建配置
├── gradle.properties                   # Gradle 属性配置
├── settings.gradle.kts                 # Gradle 设置
├── src/
│   └── main/
│       ├── java/
│       │   └── com/jiuji/plugin/codereference/
│       │       ├── action/
│       │       │   └── CopyCodeReferenceAction.java    # 复制动作实现
│       │       ├── settings/
│       │       │   ├── PathFormatType.java             # 路径格式枚举
│       │       │   ├── PluginSettings.java             # 设置状态类
│       │       │   ├── PluginSettingsState.java        # 设置持久化服务
│       │       │   └── PluginSettingsConfigurable.java # 设置页面
│       │       └── util/
│       │           └── PathFormatter.java              # 路径格式化工具类
│       └── resources/
│           └── META-INF/
│               └── plugin.xml          # 插件配置文件
└── README.md                           # 项目说明文档
```

### 构建命令

```bash
# 构建插件
./gradlew buildPlugin

# 运行插件（在沙箱环境中测试）
./gradlew runIde

# 验证插件
./gradlew verifyPlugin

# 清理构建
./gradlew clean
```

### 调试插件

1. 在 IntelliJ IDEA 中打开本项目
2. 运行 Gradle 任务 `runIde`
3. 会启动一个新的 IDE 实例，插件已自动安装
4. 在新实例中测试插件功能

## 技术实现

### 核心组件

- **CopyCodeReferenceAction**: 继承 `AnAction`，实现代码引用复制功能
- **PathFormatter**: 工具类，负责将文件路径转换为不同格式
- **PluginSettingsState**: 实现 `PersistentStateComponent`，持久化插件配置
- **PluginSettingsConfigurable**: 实现 `Configurable`，提供设置界面

### 关键技术点

1. **路径格式转换**：
   - 包路径：通过 `PsiJavaFile.getPackageName()` 获取包名
   - 相对路径：使用 `VfsUtil.getRelativePath()` 计算相对路径
   - 绝对路径：直接使用 `VirtualFile.getPath()`

2. **行号获取**：
   - 从 `Editor.getSelectionModel()` 获取选中范围
   - 使用 `Document.getLineNumber()` 转换为行号

3. **剪贴板操作**：
   - 使用 `CopyPasteManager.getInstance().setContents()` 复制到剪贴板

## 常见问题

### Q: 为什么包路径格式显示的不是包名？
A: 包路径格式仅对 Java 文件有效。对于非 Java 文件，会尝试从路径推断，如果失败则显示文件名。

### Q: 如何修改快捷键？
A: 打开 **Settings/Preferences > Keymap**，搜索 "Copy Code Reference"，右键修改快捷键。

### Q: 插件支持哪些 IDE 版本？
A: 插件支持 IntelliJ IDEA 2023.1 及以上版本（包括 Community 和 Ultimate 版本）。

## 后续计划

- [ ] 支持更多路径格式（如 GitHub URL 格式）
- [ ] 支持自定义路径前缀
- [ ] 支持批量复制多个选区
- [ ] 添加复制成功的通知提示
- [ ] 支持更多 JetBrains IDE（PyCharm、WebStorm 等）

## 许可证

本项目采用 MIT 许可证。

## 贡献

欢迎提交 Issue 和 Pull Request！

## 联系方式

- Email: support@jiuji.com
- Website: https://www.jiuji.com


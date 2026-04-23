# 电子阅读器项目说明

## 项目概述
这是一个功能完整的Android电子阅读器应用，使用Kotlin + Jetpack Compose开发，支持本地和云端数据存储。

## 技术栈
- **UI框架**: Jetpack Compose + Material Design 3
- **编程语言**: Kotlin
- **本地数据库**: Room (SQLite)
- **云端数据库**: Exposed框架 + MariaDB
- **架构模式**: MVVM (ViewModel + Repository)
- **异步处理**: Kotlin Coroutines + Flow
- **导航**: Jetpack Navigation Compose
- **数据持久化**: DataStore

## 主要功能模块

### 1. 用户认证模块
- **登录功能**: 支持用户名密码登录
- **注册功能**: 新用户注册，邮箱可选
- **用户状态管理**: 使用DataStore保存登录状态

**相关文件**:
- `viewmodel/AuthViewModel.kt`
- `ui/screens/LoginScreen.kt`
- `ui/screens/RegisterScreen.kt`
- `util/UserPreferences.kt`

### 2. 首页模块
- **推荐标签页**: 显示最新推荐书籍
- **榜单标签页**: 显示热门书籍榜单
- **书籍列表展示**: 包含书名、作者、简介、分类等信息

**相关文件**:
- `ui/screens/HomeScreen.kt`

### 3. 分类模块
- **男生分类**: 展示男生频道书籍
- **女生分类**: 展示女生频道书籍
- **标签切换**: 支持分类间快速切换

**相关文件**:
- `ui/screens/CategoryScreen.kt`

### 4. 书架模块
- **历史记录**: 显示用户阅读历史
- **收藏列表**: 显示用户收藏的书籍
- **快速访问**: 一键打开已读/收藏书籍

**相关文件**:
- `ui/screens/BookshelfScreen.kt`

### 5. 阅读模块
- **文本阅读**: 支持TXT格式书籍阅读
- **收藏功能**: 一键收藏/取消收藏
- **阅读进度**: 自动记录阅读位置（待完善）

**相关文件**:
- `ui/screens/ReaderScreen.kt`

### 6. 我的页面模块
- **个人信息展示**: 显示用户名、邮箱等
- **新增图书**: 跳转到添加书籍页面
- **退出登录**: 清除用户状态

**相关文件**:
- `ui/screens/ProfileScreen.kt`

### 7. 新增图书模块
- **TXT文件上传**: 支持从本地选择TXT文件
- **书籍信息填写**: 书名、作者、分类、简介
- **本地和云端存储**: 同时保存到本地数据库和云端（如果连接）
- **添加成功广播**: 添加书籍后发送广播通知

**相关文件**:
- `ui/screens/AddBookScreen.kt`

### 8. 阅读进度模块 ⭐新增
- **自动保存**: 每5秒自动保存阅读位置
- **退出保存**: 离开阅读页面时自动保存
- **位置恢复**: 再次打开书籍时自动跳转到上次位置
- **进度广播**: 保存进度后发送广播通知

**相关文件**:
- `viewmodel/ReadingProgressViewModel.kt`
- `ui/screens/ReaderScreen.kt`

### 9. 搜索功能模块 ⭐新增
- **实时搜索**: 输入即搜索，无需点击搜索按钮
- **多字段匹配**: 同时搜索书名、作者、简介
- **智能过滤**: 大小写不敏感搜索
- **结果统计**: 显示找到的书籍数量
- **友好提示**: 空状态和无结果状态的提示

**相关文件**:
- `ui/screens/SearchScreen.kt`

## 数据存储

### 本地数据库 (Room)
**数据表**:
- `users`: 用户信息
- `books`: 书籍信息
- `reading_progress`: 阅读进度
- `favorites`: 收藏记录

**相关文件**:
- `data/local/entity/`: Entity类
- `data/local/dao/`: DAO接口
- `data/local/database/AppDatabase.kt`: 数据库类

### 云端数据库 (Exposed + MariaDB)
**连接信息**:
- 服务器: jwli6.fun:3306
- 数据库: student
- 用户名: user
- 密码: student666666

**数据表**:
- `users`
- `books`
- `reading_progress`
- `favorites`

**相关文件**:
- `data/remote/tables/Tables.kt`: 表定义
- `data/remote/CloudDatabase.kt`: 数据库连接管理

## 广播接收器

实现了以下广播监听:
1. **自定义广播**:
   - `BOOK_ADDED`: 书籍添加
   - `BOOK_DELETED`: 书籍删除
   - `READING_PROGRESS_UPDATED`: 阅读进度更新

2. **系统广播**:
   - `ACTION_BATTERY_LOW`: 电量低提醒
   - `ACTION_POWER_CONNECTED`: 充电连接提醒

**相关文件**:
- `broadcast/BookBroadcastReceiver.kt`

## 项目结构

```
app/src/main/java/com/example/e_reader/
├── MainActivity.kt                 # 主Activity
├── broadcast/                      # 广播接收器
│   └── BookBroadcastReceiver.kt
├── data/                          # 数据层
│   ├── local/                     # 本地数据
│   │   ├── dao/                   # DAO接口
│   │   ├── database/              # 数据库类
│   │   └── entity/                # Entity类
│   ├── model/                     # 数据模型
│   ├── remote/                    # 云端数据
│   │   ├── CloudDatabase.kt
│   │   └── tables/
│   └── repository/                # Repository层
├── ui/                            # UI层
│   ├── components/                # 可复用组件
│   ├── navigation/                # 导航相关
│   ├── screens/                   # 页面
│   └── theme/                     # 主题
├── util/                          # 工具类
└── viewmodel/                     # ViewModel
```

## 如何运行

1. **环境要求**:
   - Android Studio (最新版本)
   - Kotlin 1.9+
   - Android SDK 35

2. **依赖安装**:
   项目已配置好所有依赖，首次打开会自动下载

3. **运行步骤**:
   - 打开Android Studio
   - 导入项目
   - 等待Gradle同步完成
   - 连接Android设备或启动模拟器
   - 点击Run按钮

## 功能特点

1. **美观的UI设计**: 使用Material Design 3，界面现代化
2. **完整的MVVM架构**: 代码结构清晰，易于维护
3. **双数据存储**: 本地SQLite + 云端MariaDB
4. **实时数据同步**: 使用Flow实现响应式更新
5. **广播机制**: 支持自定义和系统广播
6. **文件上传**: 支持TXT文件导入
7. **阅读体验**: 简洁的阅读界面
8. **收藏系统**: 快速收藏喜爱的书籍
9. **阅读历史**: 自动记录阅读过的书籍
10. **阅读进度保存**: 自动保存并恢复阅读位置 ⭐新增
11. **智能搜索**: 支持书名、作者、简介多字段搜索 ⭐新增

## 待优化功能

1. ~~阅读进度的详细保存和恢复~~ ✅已完成
2. ~~搜索功能的实现~~ ✅已完成
3. 书籍详情页面的完善
4. 夜间模式
5. 字体大小调节
6. 阅读主题设置
7. 云端数据同步优化
8. 图片封面支持
9. 章节管理
10. 书签功能
11. 搜索历史记录
12. 阅读时长统计
13. 阅读进度百分比显示

## 注意事项

1. 云端数据库连接需要网络权限
2. 文件上传需要存储权限
3. 广播接收器已在AndroidManifest中注册
4. 首次使用需要注册账号
5. 云端数据库连接可能需要时间，建议使用本地存储

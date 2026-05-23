# luci-app-adguardhome

![adguardhome](./images/adguardhome.png)

> [!CAUTION]
> **安全与使用须知 🔒**
> * **保护您的凭据：** **切勿**将您的配置文件或敏感信息提交到公共仓库或公开分享。
> * **默认配置：** 安装后请根据您的实际需求修改配置选项。
> * **系统要求：** 本插件需要 `adguardhome` 二进制文件作为依赖，请确保已正确安装。

## 📖 项目简介

luci-app-adguardhome 是 OpenWRT 的 LuCI 界面插件，用于管理和配置 [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome)。AdGuard Home 是一个全网广告和跟踪器屏蔽 DNS 服务器，能够有效拦截广告、跟踪器和恶意域名。

## ✨ 功能特性

- **服务管理** - 启用/禁用 AdGuard Home 服务
- **Web 界面跳转** - 一键打开 AdGuard Home 管理面板（服务运行时显示）
- **多语言支持** - 支持 4 种语言界面
  - 🇨🇳 简体中文
  - 🇹🇼 繁体中文
  - 🇯🇵 日本語
  - 🇰🇷 한국어
- **高级配置** - 支持 Go 运行时环境变量调优
  - `GOGC` - 垃圾回收器激进程度
  - `GOMAXPROCS` - 最大 OS 线程数
  - `GOMEMLIMIT` - 软内存上限
- **文件系统访问控制** - 配置只读/读写挂载路径
- **服务状态监控** - 实时显示服务运行状态和版本信息
- **详细日志** - 可选的 verbose 日志模式

## 📦 安装

### 从源码编译

1. 克隆 OpenWRT SDK 或构建环境
2. 将此仓库放置在 `package/luci-app-adguardhome/` 目录下
3. 在 `make menuconfig` 中选择：
   ```
   LuCI --->
     3. Applications --->
       <*> luci-app-adguardhome
   ```
4. 编译固件：
   ```bash
   make package/luci-app-adguardhome/compile V=s
   ```

### 依赖项

- `adguardhome` (>=0.107.73-r3)
- `luci-base`

## ⚙️ 配置说明

### 常规设置

| 选项 | 说明 | 默认值 |
|------|------|--------|
| 启用服务 | 启动 AdGuard Home 服务 | 禁用 |
| 配置文件 | AdGuard Home 配置文件路径 | `/etc/adguardhome/adguardhome.yaml` |
| 工作目录 | 存储过滤器、日志和统计信息的目录 | `/var/lib/adguardhome` |
| 服务用户 | 服务运行用户 | `adguardhome` |
| 服务用户组 | 服务运行用户组 | `adguardhome` |
| 详细日志 | 启用详细日志输出 | 禁用 |

### 高级设置

> [!WARNING]
> **风险提示 ⚠️**
> * 高级设置仅适用于有经验的用户
> * 错误的配置可能导致服务无法启动或性能问题
> * 修改前请备份原始配置

| 选项 | 说明 | 默认值 |
|------|------|--------|
| GOGC | 垃圾回收器激进程度百分比 | 未设置 (100) |
| GOMAXPROCS | 最大 OS 线程数 | 未设置 (CPU 核心数) |
| GOMEMLIMIT | 软内存上限 (MB) | 未设置 (禁用) |

### 文件系统访问

| 选项 | 说明 |
|------|------|
| 只读访问 | AdGuard Home 需要只读访问的文件/目录列表 |
| 读写访问 | AdGuard Home 需要读写访问的文件/目录列表 |

## 🚀 使用方法

1. **安装插件** - 通过 opkg 或从源码编译安装
2. **启用服务** - 进入 LuCI → 服务 → AdGuard Home → 勾选"启用服务"
3. **保存并应用** - 点击保存并应用配置
4. **启动服务** - 服务将在系统启动时自动运行
5. **访问 Web 界面** - 服务运行后，点击"启动 Web 界面"按钮打开 AdGuard Home 管理面板

> [!TIP]
> **快速访问 💡**
> * AdGuard Home Web 界面默认运行在 `http://<路由器 IP>:3000`
> * 您也可以直接访问该地址，无需通过 LuCI 跳转

## 📝 配置文件示例

```ini
config adguardhome 'config'
	option enabled '1'
	option config_file '/etc/adguardhome/adguardhome.yaml'
	option work_dir '/var/lib/adguardhome'
	option user 'adguardhome'
	option group 'adguardhome'
	option verbose '0'
	option gc '0'
	option maxprocs '0'
	option memlimit '0'
```

## 🔧 开发信息

### 项目结构

```
luci-app-adguardhome/
├── htdocs/
│   └── luci-static/
│       └── resources/
│           └── view/
│               └── adguardhome/
│                   └── config.js          # LuCI 前端界面
├── po/
│   ├── templates/
│   │   └── adguardhome.pot               # 翻译模板
│   ├── zh_Hans/
│   │   └── adguardhome.po                # 简体中文
│   ├── zh_Hant/
│   │   └── adguardhome.po                # 繁体中文
│   ├── ja/
│   │   └── adguardhome.po                # 日语
│   └── ko/
│       └── adguardhome.po                # 韩语
└── root/
    └── usr/
        └── share/
            ├── luci/
            │   └── menu.d/
            │       └── luci-app-adguardhome.json
            ├── rpcd/
            │   └── acl.d/
            │       └── luci-app-adguardhome.json
            └── ucitrack/
                └── luci-app-adguardhome.json
```

### 许可证

- **GPL-2.0-only**

### 维护者

- George Sapkin <george@sapk.in>
- MomoFlora <2519840456@qq.com>

## 🔗 相关链接

- [AdGuard Home 官方仓库](https://github.com/AdguardTeam/AdGuardHome)
- [OpenWRT 软件包](https://github.com/openwrt/packages)
- [LuCI 项目](https://github.com/openwrt/luci)

# luci-app-adguardhome

> [!CAUTION]
> **Security & Usage Notice 🔒**
> * **PROTECT YOUR CREDENTIALS:** **Never** commit your configuration files or sensitive information to public repositories or share them openly. 
> * **DEFAULT CONFIGURATION:** Please modify the configuration options according to your actual needs after installation.
> * **SYSTEM REQUIREMENTS:** This plugin requires the `adguardhome` binary as a dependency. Ensure it is properly installed.

## 📖 Overview

luci-app-adguardhome is a LuCI interface plugin for OpenWRT, used to manage and configure [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome). AdGuard Home is a network-wide ad and tracker blocking DNS server that effectively intercepts ads, trackers, and malicious domains.

## ✨ Features

- **Service Management** - Enable/disable AdGuard Home service
- **Web Interface Access** - One-click to open AdGuard Home admin panel (shown when service is running)
- **Multi-language Support** - Supports 4 language interfaces
  - 🇨🇳 简体中文 (Simplified Chinese)
  - 🇹🇼 繁體中文 (Traditional Chinese)
  - 🇯🇵 日本語 (Japanese)
  - 🇰🇷 한국어 (Korean)
- **Advanced Configuration** - Supports Go runtime environment variable tuning
  - `GOGC` - Garbage collector aggressiveness
  - `GOMAXPROCS` - Maximum OS threads
  - `GOMEMLIMIT` - Soft memory limit
- **File System Access Control** - Configure read-only/read-write mount paths
- **Service Status Monitoring** - Real-time service status and version display
- **Verbose Logging** - Optional verbose logging mode

## 📦 Installation

### Build from Source

1. Clone OpenWRT SDK or build environment
2. Place this repository under `package/luci-app-adguardhome/`
3. Select in `make menuconfig`:
   ```
   LuCI --->
     3. Applications --->
       <*> luci-app-adguardhome
   ```
4. Build the firmware:
   ```bash
   make package/luci-app-adguardhome/compile V=s
   ```

### Dependencies

- `adguardhome` (>=0.107.73-r3)
- `luci-base`

## ⚙️ Configuration

### General Settings

| Option | Description | Default Value |
|--------|-------------|---------------|
| Enable Service | Start AdGuard Home service | Disabled |
| Configuration file | AdGuard Home config file path | `/etc/adguardhome/adguardhome.yaml` |
| Working directory | Directory for filters, logs, and statistics | `/var/lib/adguardhome` |
| Service user | User the service runs under | `adguardhome` |
| Service group | Group the service runs under | `adguardhome` |
| Verbose logging | Enable verbose log output | Disabled |

### Advanced Settings

> [!WARNING]
> **Risk Warning ⚠️**
> * Advanced settings are for experienced users only
> * Incorrect configuration may cause service failure or performance issues
> * Backup your original configuration before making changes

| Option | Description | Default Value |
|--------|-------------|---------------|
| GOGC | Garbage collector aggressiveness percentage | Unset (100) |
| GOMAXPROCS | Maximum OS threads | Unset (CPU cores) |
| GOMEMLIMIT | Soft memory limit (MB) | Unset (disabled) |

### File System Access

| Option | Description |
|--------|-------------|
| Read-only access | List of files/directories AdGuard Home needs read-only access to |
| Read-write access | List of files/directories AdGuard Home needs read-write access to |

## 🚀 Usage

1. **Install Plugin** - Install via opkg or compile from source
2. **Enable Service** - Navigate to LuCI → Services → AdGuard Home → Check "Enable Service"
3. **Save & Apply** - Click Save & Apply to save configuration
4. **Start Service** - Service will start automatically on system boot
5. **Access Web Interface** - When service is running, click "Launch Web Interface" button to open AdGuard Home admin panel

> [!TIP]
> **Quick Access 💡**
> * AdGuard Home Web Interface runs by default at `http://<router IP>:3000`
> * You can also access this address directly without LuCI redirection

## 📝 Configuration File Example

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

## 🔧 Development Info

### Project Structure

```
luci-app-adguardhome/
├── htdocs/
│   └── luci-static/
│       └── resources/
│           └── view/
│               └── adguardhome/
│                   └── config.js          # LuCI frontend interface
├── po/
│   ├── templates/
│   │   └── adguardhome.pot               # Translation template
│   ├── zh_Hans/
│   │   └── adguardhome.po                # Simplified Chinese
│   ├── zh_Hant/
│   │   └── adguardhome.po                # Traditional Chinese
│   ├── ja/
│   │   └── adguardhome.po                # Japanese
│   └── ko/
│       └── adguardhome.po                # Korean
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

### License

- **GPL-2.0-only**

### Maintainer

- George Sapkin <george@sapk.in>
- MomoFlora <2519840456@qq.com>

## 🔗 Related Links

- [AdGuard Home Official Repository](https://github.com/AdguardTeam/AdGuardHome)
- [OpenWRT Packages](https://github.com/openwrt/packages)
- [LuCI Project](https://github.com/openwrt/luci)

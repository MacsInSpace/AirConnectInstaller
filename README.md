# AirConnect Installer for macOS

A standalone installer script for [AirConnect](https://github.com/philippe44/AirConnect) that automatically downloads the latest release and sets up launchd services on macOS.

## What is AirConnect?

AirConnect adds AirPlay capabilities to Chromecast and UPnP (like Sonos) players, making them appear as AirPlay devices. This installer makes it easy to install and keep AirConnect updated on macOS.

**Note:** AirConnect is developed by [philippe44](https://github.com/philippe44/AirConnect). This is just an installer script to make installation easier on macOS.

## Features

- ✅ Automatically downloads latest AirConnect release
- ✅ Detects macOS architecture (Intel/Apple Silicon)
- ✅ Sets up launchd services for automatic startup
- ✅ Configures OpenSSL symlinks when Homebrew is installed
- ✅ Uses dynamic binaries (recommended) when OpenSSL is available
- ✅ Falls back to static binaries if needed
- ✅ Handles both new and legacy service names

## Installation

### Option 1: Direct Install (curl)

```bash
curl -sL https://raw.githubusercontent.com/MacsInSpace/AirConnectInstaller/main/install.sh | sudo bash
```

### Option 2: Homebrew (Recommended)

```bash
brew tap MacsInSpace/airconnect
brew install airconnect-installer
sudo airconnect-install
```

### Option 3: Manual Install

1. Download `install.sh`
2. Make it executable: `chmod +x install.sh`
3. Run with sudo: `sudo ./install.sh`

## Requirements

- macOS (Intel or Apple Silicon)
- Root/sudo access
- Internet connection (to download AirConnect)

**Optional but recommended:**
- Homebrew (for OpenSSL support to use dynamic binaries)

## What Gets Installed

- **Binaries:** `/usr/local/bin/aircast` and `/usr/local/bin/airupnp`
- **Services:** LaunchAgent plists in `/Library/LaunchAgents/`
- **Logs:** `/var/log/aircast.log` and `/var/log/airupnp.log`

## Usage

After installation, AirConnect will start automatically. You should see new AirPlay devices appear in your system.

### View Logs

```bash
tail -f /var/log/aircast.log
tail -f /var/log/airupnp.log
```

### Stop Services

```bash
launchctl bootout gui/$(id -u)/com.aircast.plist
launchctl bootout gui/$(id -u)/com.airupnp.plist
```

### Start Services

```bash
launchctl bootstrap gui/$(id -u) /Library/LaunchAgents/com.aircast.plist
launchctl bootstrap gui/$(id -u) /Library/LaunchAgents/com.airupnp.plist
```

### Uninstall

```bash
# Stop services
launchctl bootout gui/$(id -u)/com.aircast.plist 2>/dev/null
launchctl bootout gui/$(id -u)/com.airupnp.plist 2>/dev/null

# Remove files
sudo rm -f /usr/local/bin/aircast /usr/local/bin/airupnp
sudo rm -f /Library/LaunchAgents/com.aircast.plist /Library/LaunchAgents/com.airupnp.plist
sudo rm -f /var/log/aircast.log /var/log/airupnp.log
```

## OpenSSL Support

If you have Homebrew installed, the installer will:
1. Detect OpenSSL installation
2. Create symlinks in `/usr/local/lib/` if needed
3. Use dynamic binaries (recommended) instead of static ones

This provides better compatibility and smaller binary sizes.

## Troubleshooting

### Services won't start
- Check logs: `tail -f /var/log/aircast.log`
- Verify binaries exist: `ls -la /usr/local/bin/aircast`
- Check plist syntax: `plutil -lint /Library/LaunchAgents/com.aircast.plist`

### Can't find AirPlay devices
- Ensure AirConnect is running: `ps aux | grep aircast`
- Check firewall settings (port 5353 UDP needed)
- Verify network connectivity

### OpenSSL issues
- Install Homebrew: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
- Install OpenSSL: `brew install openssl`
- Re-run installer

## Updating

Simply run the installer again - it will download the latest version and update your installation.

## License

This installer script is provided as-is. AirConnect itself is licensed separately by [philippe44](https://github.com/philippe44/AirConnect).

## Issues

- **AirConnect software issues:** [philippe44/AirConnect Issues](https://github.com/philippe44/AirConnect/issues)
- **Installer issues:** [MacsInSpace/AirConnectInstaller Issues](https://github.com/MacsInSpace/AirConnectInstaller/issues)

## Credits

- **AirConnect:** [philippe44](https://github.com/philippe44/AirConnect)
- **Installer:** [MacsInSpace](https://github.com/MacsInSpace)

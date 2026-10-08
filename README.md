# hib CLI releases

本仓库提供私有源码仓库 `hib-cli` 的公开二进制发布包和安装器。

## 安装

### macOS、Linux 或 WSL

```sh
curl -fsSL https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.sh | sh
```

### Windows PowerShell

```powershell
irm https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.ps1 | iex
```

安装器会识别操作系统和处理器架构、校验 SHA-256，并在验证新二进制可以启动后替换原版本。默认安装到 Linux/macOS 的 `~/.local/bin/hib` 或 Windows 的 `%LOCALAPPDATA%\Programs\hib\bin\hib.exe`；可通过 `HIB_INSTALL_DIR` 修改位置。

若 Unix 安装目录不在 `PATH`，安装器会自动更新 bash、zsh 或 fish 的启动配置，新终端自动生效；当前终端需重新打开，或执行安装器输出的 `export PATH=...` 命令。Windows 安装器会更新用户 `PATH`，新终端自动生效。

## 更新

重复执行安装命令即可安装最新版本：

macOS、Linux 或 WSL 执行：

```bash
curl -fsSL https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.sh | sh
```

Windows PowerShell 执行：

```powershell
irm https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.ps1 | iex
```

设置过 `HIB_INSTALL_DIR` 时，更新前需设置相同的环境变量。

## 卸载

macOS、Linux 或 WSL 重新执行安装器并传入 `--uninstall`：

```bash
curl -fsSL https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.sh | sh -s -- --uninstall
```

Windows PowerShell 执行：

```powershell
& ([scriptblock]::Create((irm https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.ps1))) -Uninstall
```

默认卸载会删除程序以及安装器写入的 `PATH` 配置，本地登录信息仍保留在 `~/.hib`。如需连同登录信息彻底清理，将 Unix 的 `--uninstall` 改为 `--purge`，或将 PowerShell 的 `-Uninstall` 改为 `-Purge`。设置过 `HIB_INSTALL_DIR` 时，卸载前需设置相同的环境变量。

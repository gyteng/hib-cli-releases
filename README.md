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

### Windows CMD

```bat
powershell -NoProfile -Command "irm https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.ps1 | iex"
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

Windows CMD 执行：

```bat
powershell -NoProfile -Command "irm https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.ps1 | iex"
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

Windows CMD 执行：

```bat
powershell -NoProfile -Command "& ([scriptblock]::Create((irm https://github.com/gyteng/hib-cli-releases/releases/latest/download/install.ps1))) -Uninstall"
```

默认卸载会删除程序以及安装器写入的 `PATH` 配置，本地登录信息仍保留在 `~/.hib`。如需连同登录信息彻底清理，将 Unix 的 `--uninstall` 改为 `--purge`，或将 Windows PowerShell / CMD 命令中的 `-Uninstall` 改为 `-Purge`。设置过 `HIB_INSTALL_DIR` 时，卸载前需设置相同的环境变量。

## CI 与发布（维护者）

本仓库收到 `v*` tag 推送后，从私有 `gyteng/hib-cli` 仓库检出同名 tag，执行测试及 Windows x64、Linux x64/arm64、macOS x64/arm64 构建并发布。私有源码仓库不运行 Actions。

1. 在本仓库 Actions secrets 配置 `SOURCE_READ_TOKEN`，仅授予私有源码仓库 Contents: read。发布使用本仓库 `GITHUB_TOKEN`。
2. 先将工作流提交到 main 分支。
3. 在私有源码仓库创建并推送版本 tag，再在本仓库创建并推送同名 tag。两个仓库的 tag 指向各自的提交；本仓库 tag 必须包含发布工作流。

```powershell
git -C C:\Users\HM\Code\hib-cli tag v0.1.26
git -C C:\Users\HM\Code\hib-cli push origin refs/tags/v0.1.26
git -C C:\Users\HM\Code\hib-cli-releases tag v0.1.26
git -C C:\Users\HM\Code\hib-cli-releases push origin refs/tags/v0.1.26
```

上述创建 tag 命令适用于尚不存在的 tag。临时故障导致构建失败时，可在本仓库 Actions 页面重跑，无需移动 tag。重跑使用原 tag 的工作流和源码；如果修复了工作流或源码，需要提交修复并创建新的版本及两个仓库的同名 tag，重跑旧任务不会应用新提交。

源码 tag 不存在或与 package.json 版本不匹配时终止。全部五个平台通过后生成 SHA256SUMS 并正式发布；失败可重试草稿，不能覆盖已正式发布的版本。

源码测试和二进制测试套件保留更新检查的默认行为，以覆盖更新提醒等逻辑；`HIB_NO_UPDATE_CHECK=1` 仅用于直接运行二进制的帮助和版本检查，避免这些命令触发真实更新请求。

原始测试和构建日志不公开上传，也不缓存源码；失败时在本机检出对应源码 tag 复现。工作流、执行元数据与正式发布内容公开可见。仅允许可信维护者修改工作流与推送版本 tag。

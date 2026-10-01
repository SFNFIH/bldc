# VESC 固件

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Travis CI Status](https://travis-ci.com/vedderb/bldc.svg?branch=master)](https://travis-ci.com/vedderb/bldc)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/75e90ffbd46841a3a7be2a9f7a94c242)](https://www.codacy.com/app/vedderb/bldc?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=vedderb/bldc&amp;utm_campaign=Badge_Grade)
[![Contributors](https://img.shields.io/github/contributors/vedderb/bldc.svg)](https://github.com/vedderb/bldc/graphs/contributors)
[![Watchers](https://img.shields.io/github/watchers/vedderb/bldc.svg)](https://github.com/vedderb/bldc/watchers)
[![Stars](https://img.shields.io/github/stars/vedderb/bldc.svg)](https://github.com/vedderb/bldc/stargazers)
[![Forks](https://img.shields.io/github/forks/vedderb/bldc.svg)](https://github.com/vedderb/bldc/network/members)

一款开源电机控制器固件。

这是 VESC DC/BLDC/FOC 控制器的源代码。更多信息请访问
[https://vesc-project.com/](https://vesc-project.com/)

## 支持的板卡

全都支持！

输入 `make` 查看支持的板卡：

```
[Firmware]
     fw   - Build firmware for default target
                            supported boards are: 100_250 100_250_no_limits 100_500...
```

还有许多其他选项可在 [conf_general.h](conf_general.h) 中修改。

## 前置要求

### Ubuntu (Linux) / macOS
- 工具：`git`、`wget` 和 `make`
- Linux 额外依赖：`libgl-dev` 和 `libxcb-xinerama0`
- 实用的 Ubuntu 安装命令：
```bash
sudo apt install git build-essential libgl-dev libxcb-xinerama0 wget git-gui
```
- 实用的 macOS 工具：

```bash
brew install stlink
brew install openocd
```

### Windows
- Chocolatey：https://chocolatey.org/install
- Git：https://git-scm.com/download/win。请勾选将 Git 添加到环境变量（PATH）的选项。

## 安装开发环境并编译

### Ubuntu (Linux) / macOS
打开终端
1.  `git clone http://github.com/vedderb/bldc.git`
2.  `cd bldc`
3.  继续执行 [所有平台通用步骤](#所有平台通用步骤)

### Windows

1.  打开 Windows PowerShell 终端（不要以管理员身份运行 PowerShell，否则会出问题）
2.  输入 `choco install make`
3.  `git clone http://github.com/vedderb/bldc`
4.  `cd bldc`
5.  继续执行 [所有平台通用步骤](#所有平台通用步骤)

### 所有平台通用步骤

1.  `git checkout origin/master`
2.  `make arm_sdk_install`
3.  `make` ← 从支持的板卡列表中选择目标设备名称。例如，我有一块 Trampa **VESC 100/250**，目标就是 `100_250`
4.  `make 100_250` ← 这会编译 **VESC 100/250** 固件，并输出到 `bldc/builds/100_250/` 目录

## 其他工具

**Linux 可选 — 添加 udev 规则，以便无需 root 权限即可使用 stlink v2 烧录器**
```bash
wget vedder.se/Temp/49-stlinkv2.rules
sudo mv 49-stlinkv2.rules /etc/udev/rules.d/
sudo udevadm trigger
```

## IDE
### 前置要求
#### macOS / Linux

- `python3` 和 `pip`

#### Windows
- Python 3：https://www.python.org/downloads/。请勾选将 Python3 添加到环境变量的选项。

### 所有平台

1.  `pip install aqtinstall`
2.  `make qt_install`
3.  打开安装在 `tools/Qt/Tools/QtCreator/bin/qtcreator` 的 Qt Creator IDE
4.  在 Qt Creator 中打开 vesc 固件项目 `vesc.pro`，路径为 `Project/Qt Creator/vesc.pro`
5.  IDE 默认配置为编译 100_250 固件；可在左侧面板底部更改，那里列出了 VESC 支持的所有硬件变体

## 上传到 VESC
### 方法 1 — 使用 STLink SWD 调试器烧录

1.  先编译并烧录 [bootloader](https://github.com/vedderb/bldc-bootloader)
2.  然后对目标执行 `_flash`。例如 VESC 100/250：
```bash
make 100_250_flash
```

### 方法 2 — 通过 USB 使用 VESC Tool 上传固件

1.  按上述编译说明，克隆并编译出 **.bin** 格式固件

在 VESC Tool 中：

2.  连接到 VESC
3.  在左侧菜单进入 Firmware（固件）选项卡
4.  点击 Custom file（自定义文件）选项卡
5.  点击文件夹图标，选择编译好的 .bin 固件（例如 `build/100_250/100_250.bin`）

##### [ 提醒：固件上传过程中 VESC 断开连接是正常现象 ]
#####  **[ 警告：上传过程中切勿断开 VESC 的电源/USB，否则可能变砖 ]**
#####  **[ 警告：仅在进度条完成并显示 “FW Upload DONE” 后再等待 10 秒，才可断开 VESC ]**

6.  点击右下角的上传固件按钮（向下箭头），开始上传所选固件。
7.  进度条完成后请再等待 **10 秒**（警告：过早拔线可能导致变砖）
8.  新固件上传完成后，VESC 会自行断开连接。

## 如果 VESC 变砖了
你需要向 VESC 上传一份可用的新固件。
但对已变砖的 VESC 上传固件，必须使用 SWD 调试器。


## 参与贡献

前往 [论坛](https://vesc-project.com/forum) 参与并改进本项目。
加入 [Discord](https://discord.gg/JgvV5NwYts) 获取实时支持与交流。

## 标签（Tags）

每个固件版本都有对应标签，创建方式如下：

```bash
git tag -a [version] [commit] -m "VESC Firmware Version [version]"
git push --tags
```

## 许可证

本软件以 GNU General Public License version 3.0 发布。

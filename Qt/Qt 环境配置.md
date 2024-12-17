---
title: Qt环境配置
aliases: 
author: SprInec
date: 2024-12-16
update: 2024-12-16 20:48:20
reference: 
tags:
  - Qt
---
# Qt 环境配置

## 一. 交叉编译环境配置

### 1.1 Windows 下的 Ubuntu 虚拟机 Qt 交叉编译环境构建

在 Windows 平台上，通过 Ubuntu 虚拟机（WSL 或 VMWare）构建一个 ARM 的 Qt 交叉编译环境，主要包括以下几个步骤：

#### 1.1.1 安装交叉编译工具链

首先，需要在 Ubuntu 虚拟机 上安装 ARM 交叉编译工具链。常用的 ARM 交叉编译工具链包括 `gcc-arm-linux-gnueabihf`（用于 ARMv7 架构）和 `gcc-arm-linux-gnueaarch64`（用于 ARMv8 架构）。这里以 ARMv7 为例。

```bash
sudo apt update
sudo apt install gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
```

如果需要 ARMv8 交叉编译工具链，可以安装：

```bash
sudo apt install gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
```

#### 1.1.2 安装 Qt 源代码

下载 Qt 的源代码，可以从 Qt 官方下载或者直接从 GitHub 克隆：

```bash
git clone https://code.qt.io/qt/qt5.git --branch 5.15 --single-branch
cd qt5
git submodule update --init --recursive
```

>[!Note] 
> 
> `git submodule update --init --recursive` 这条命令用于初始化并更新 Git 仓库中的 **子模块**。子模块是 Git 中的一种机制，用于在一个 Git 仓库中引用另一个 Git 仓库（子仓库），通常用于管理大型项目中依赖的外部代码库。
#### 1.1.3 配置 Qt 构建环境

在交叉编译之前，需要配置 Qt，指定使用 ARM 工具链来进行编译。可以通过设置工具链文件来完成。

创建一个新的配置文件（例如 `arm-qt5.config`），并在该文件中设置交叉编译的工具链信息：

```bash
cat > arm-qt5.config <<EOF
QMAKE_CC = arm-linux-gnueabihf-gcc
QMAKE_CXX = arm-linux-gnueabihf-g++
QMAKE_LINK = arm-linux-gnueabihf-g++
QMAKE_AR = arm-linux-gnueabihf-ar
QMAKE_NM = arm-linux-gnueabihf-nm
QMAKE_OBJCOPY = arm-linux-gnueabihf-objcopy
QMAKE_OBJDUMP = arm-linux-gnueabihf-objdump
QMAKE_STRIP = arm-linux-gnueabihf-strip
EOF
```

确保这个工具链文件指向正确的 ARM 交叉编译工具。如果使用的是 ARMv8（aarch64），则应使用 `aarch64-linux-gnu-*` 工具链。

#### 1.1.4 配置 Qt 使用交叉编译工具链

在 Qt 源代码目录下运行以下命令来配置交叉编译环境。

```bash
./configure -xplatform aarch64-none-linux-gnu-g++ \
            -prefix /path/to/your/arm/qt/install/dir \
            -opensource \
            -confirm-license \
            -optimized-qmake \
            -no-pch \
            -nomake examples \
            -nomake tests \
            -make libs
```

- `-xplatform linux-arm-gnueabihf-g++`：指定交叉编译的工具链。
- `-prefix`：指定安装目录。
- `-opensource`：使用开源许可证。
- `-optimized-qmake`：优化 qmake 的构建速度。
- `-nomake examples`、`-nomake tests`：避免编译示例和测试，节省时间。
- `-make libs`：只编译 Qt 库。

#### 1.1.5 构建 Qt

配置完成后，使用 `make` 开始编译：

```bash
make -j$(nproc)
```

此命令将并行编译 Qt，`-j$(nproc)` 会自动检测 CPU 核心数并加速编译过程。

#### 1.1.6 安装 Qt

编译完成后，可以将 Qt 安装到指定的目录（在 `-prefix` 参数中设置的路径）：

```bash
make install
```

#### 1.1.7 配置 Qt Creator（可选）

如果希望使用 Qt Creator 来开发和调试 ARM 平台上的应用程序，需要在 Qt Creator 中配置交叉编译工具链。

- 打开 Qt Creator，进入 **工具** > **选项** > **Kits**。
- 配置交叉编译工具链：
    - **C++ 编译器**：选择 ARM 工具链中的 `g++`。
    - **C 编译器**：选择 ARM 工具链中的 `gcc`。
    - **调试器**：使用适合目标平台的调试器（例如 `gdb-multiarch`）。
    - **Qt 版本**：选择你安装的 ARM 版 Qt。

通过这些步骤，就能在 Ubuntu 虚拟机（WSL 或 VMWare）上搭建 ARM 平台的 Qt 交叉编译环境，进行 ARM 应用开发。

### 1.2 Windows 的 Qt 交叉编译环境构建
#### 1.2.1 安装交叉编译工具链

在 Windows 上，可以使用 **MinGW** 或 **LLVM** 等工具链来进行 ARM 的交叉编译。最常见的交叉编译工具链是 **ARM 官方提供的工具链**，比如 `gcc-arm-none-eabi` 或 `gcc-arm-linux-gnueabihf`。你可以通过以下方法安装：

安装 **ARM 工具链**：

可以通过以下几种方式在 Windows 上安装 ARM 工具链：

- **通过 `wsl` 安装**：如果已经使用了 WSL（Windows Subsystem for Linux），可以直接在 WSL 中安装交叉编译工具链，然后将工具链与 Windows 系统共享。
    
    进入 WSL 环境，执行：
    
    ```bash
    sudo apt update
    sudo apt install gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
    ```
    
- **直接从 ARM 官网下载工具链**：可以从 ARM 官方网站下载适用于 Windows 的工具链 [ARM Tools](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm) 并安装。下载完后，需要将工具链的路径添加到系统环境变量中。

#### 1.2.2 安装 Qt 和交叉编译配置

需要从 Qt 官网或 GitHub 获取源代码，并设置交叉编译工具链。

##### 1. 下载 Qt 源代码

可以通过 Git 克隆 Qt 源码，也可以从 Qt 官网下载最新版本的 Qt。

```bash
git clone https://code.qt.io/qt/qt5.git --branch 5.15 --single-branch
cd qt5
git submodule update --init --recursive
```

>[!Note] 
> 
> `git submodule update --init --recursive` 这条命令用于初始化并更新 Git 仓库中的 **子模块**。子模块是 Git 中的一种机制，用于在一个 Git 仓库中引用另一个 Git 仓库（子仓库），通常用于管理大型项目中依赖的外部代码库。
##### 2. 配置 Qt 使用 ARM 工具链

在 Qt 源代码目录下，执行以下命令来配置 Qt 使用 ARM 工具链进行交叉编译。首先，确保已经下载并安装了 ARM 工具链。

创建一个工具链配置文件（例如 `arm-qt5.config`），其中指定交叉编译所使用的工具链：

```powershell
@"
QMAKE_STRIP = aarch64-none-linux-gnu-strip
QMAKE_OBJDUMP = aarch64-none-linux-gnu-objdump
QMAKE_OBJCOPY = aarch64-none-linux-gnu-objcopy
QMAKE_NM = aarch64-none-linux-gnu-nm
QMAKE_AR = aarch64-none-linux-gnu-ar
QMAKE_LINK = aarch64-none-linux-gnu-g++
QMAKE_CXX = aarch64-none-linux-gnu-g++
QMAKE_CC = aarch64-none-linux-gnu-gcc
"@ > arm-qt5.config
```

##### 3. 配置 Qt

在 Qt 源代码目录下运行以下配置命令：

```powershell
./configure -xplatform aarch64-none-linux-gnu-g++ `
            -prefix "F:\Packages\qt5-arm" `
            -opensource `
            -confirm-license `
            -optimized-qmake `
            -no-pch `
            -nomake examples `
            -nomake tests `
            -make libs
```

- `-xplatform aarch64-none-linux-gnu-g++`：指定交叉编译工具链。
- `-prefix`：指定安装目录。
- `-opensource`：使用开源许可证。
- `-optimized-qmake`：优化 qmake 构建速度。
- `-nomake examples`、`-nomake tests`：避免编译示例和测试，节省时间。
- `-make libs`：只编译 Qt 库。

>[!Note] 
> 
> Qt 配置脚本通常需要 [Perl](https://strawberryperl.com/) 来执行一些任务，因此需要确保 Perl 已安装并正确配置到 PATH 环境变量中。

#### 1.2.3 使用 MinGW 构建 Qt

如果不想通过 `wsl` 来构建，可以使用 **MinGW** 或 **MSYS2** 提供的交叉编译工具链，具体步骤如下：

##### 1. 安装 MinGW-w64 工具链

1. 下载并安装 **MSYS2**，然后在 MSYS2 环境中安装交叉编译工具链：
    
    ```bash
    pacman -S mingw-w64-x86_64-gcc
    ```
    
2. 配置 MinGW 的交叉编译环境：
    
    编辑 `qmake.conf` 文件（位于 `mkspecs` 目录下），以指向 MinGW 工具链。
    
3. 运行 Qt 配置命令进行交叉编译。

##### 2. 安装 CMake 和 Ninja（可选）

如果选择使用 CMake 来编译 Qt，建议在 Windows 上安装 Ninja 构建工具，以加速构建过程。

```bash
choco install cmake ninja
```

#### 1.2.4 构建 Qt

配置完成后，开始构建 Qt：

```bash
make -j$(nproc)
```

这个命令将会并行编译 Qt，`-j$(nproc)` 会自动检测 CPU 核心数来加速编译过程。

#### 1.2.5 安装 Qt

编译完成后，使用以下命令安装 Qt 到指定目录：

```bash
make install
```

#### 1.2.6 配置 Qt Creator（可选）

如果希望使用 Qt Creator 来开发 ARM 平台上的应用程序，可以配置 Qt Creator：

1. 打开 Qt Creator，进入 **工具** > **选项** > **Kits**。
2. 配置交叉编译工具链：
    - **C++ 编译器**：选择 ARM 工具链中的 `g++`。
    - **C 编译器**：选择 ARM 工具链中的 `gcc`。
    - **调试器**：使用适合目标平台的调试器（例如 `gdb-multiarch`）。
    - **Qt 版本**：选择你安装的 ARM 版 Qt。

这样，就可以在 Windows 环境下进行 ARM 平台的 Qt 应用开发了。


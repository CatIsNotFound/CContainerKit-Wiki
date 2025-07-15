# 项目开发

## 项目简介

本项目是一个 C 语言开发的容器库，旨在提供一组通用的数据结构和算法实现，以满足不同场景下的需求。

## 项目目标

本项目的目标是提供一个易用、高效、可扩展的容器库，以满足不同场景下的需求。

## 项目结构

本项目的结构如下：

```bash
CContainerKit/
├── include/           # 头文件目录
│   ├── CContainerKit/  # 容器库头文件目录
│   │   ├── CArray.h   # 数组容器头文件
│   │   ├── CList.h    # 链表容器头文件
│   │   ├── CString.h  # 字符串容器头文件
│   │   ├── CVariant.h # 通用数据类型头文件
│   │   └── ...        # 其他头文件
│   ├── CContainerKit.h # 容器库总头文件
├── src/               # 源文件目录
│   ├── CMakeLists.txt  # CMake 配置文件
│   ├── CArray.c       # 数组容器源文件
│   ├── CList.c        # 链表容器源文件
│   ├── CString.c      # 字符串容器源文件
│   ├── CVariant.c     # 通用数据类型源文件
│   └── ...            # 其他源文件
├── test/              # 测试文件目录
│   ├── unit_tests     # 单元测试文件目录
│   │   ├── CArray_test.c   # 数组容器单元测试文件
│   │   ├── CList_test.c    # 链表容器单元测试文件
│   │   ├── CString_test.c  # 字符串容器单元测试文件
│   │   ├── CVariant_test.c # 通用数据类型单元测试文件
│   ├── main.c         # 测试主程序文件
│   └── CMakeLists.txt      # CMake 配置文件
├── README.md          # 项目说明文件
├── LICENSE            # 许可证文件
└── CMakeLists.txt     # CMake 配置文件
```

## 编译项目

本项目使用 CMake 进行编译。具体编译方式由如下两种：

- 使用 IDE 集成编译：通过内置构建工具轻松安装和配置项目。
- 手动编译：通过命令行手动编译项目。但需要手动安装构建工具。

### IDE 集成编译

#### Visual Studio

1. 打开 Visual Studio，选择 `File` -> `Open` -> `Project/Solution`。
2. 选择项目目录下的 `CMakeLists.txt` 文件。
3. 选择 `CMake` 作为生成器，点击 `Open`。
4. 等待 Visual Studio 加载项目。
5. 选择 `Build` -> `Build All` 或使用快捷键 `Ctrl + Shift + B` 来编译项目。

#### CLion

1. 打开 CLion，选择 `File` -> `Open`。
2. 选择项目目录。
3. 等待 CLion 加载项目。
4. 选择 `Build` -> `Build Project` 或使用快捷键 `Ctrl + F9` 来编译项目。

#### Code::Blocks

1. 打开 Code::Blocks，选择 `File` -> `Open`。
2. 选择项目目录。
3. 等待 Code::Blocks 加载项目。
4. 选择 `Build` -> `Build and Run` 或使用快捷键 `F9` 来编译项目。

### 手动编译

若需要手动编译项目，请根据不同的编译器进行操作。

#### Ninja

!!! note
    在使用此构建方法前，请先单独安装 Ninja。
    检查终端是否已安装 Ninja:
    ```bash
    ninja --version
    ```
    Windows 环境下，需要**手动配置环境变量**才能使用。

1. 打开终端，进入项目目录。
2. 执行以下命令编译项目：
   ```bash
   mkdir build
   cd build
   cmake.. -G "Ninja"
   ninja
   ```

#### GCC

!!! note
    Linux 环境下，需要手动使用包管理器进行安装。
    例如：在 Debian 环境下，需要执行：
    ```bash
    sudo apt install gcc gdb valgrind make cmake -y
    ```

1. 打开终端，进入项目目录。
2. 执行以下命令编译项目：
   ```bash
   mkdir build
   cd build
   cmake.. -G "Unix Makefiles"
   make
   ```

#### MinGW

!!! note
    Windows 环境下，需要手动安装 MinGW。关于其 MinGW 的下载，请自行在网上搜索。
    
    安装完成后，需要**手动配置环境变量**才能使用。
    检查终端是否已安装 MinGW：
    ```bash
    gcc --version
    ```

1. 打开终端，进入项目目录。
2. 执行以下命令编译项目：
   ```bash
   mkdir build
   cd build
   cmake .. -G "MinGW Makefiles"
   mingw32-make.exe
   ```


## 代码规范

详见[代码开发规范](./codes.md)。

## 如何贡献？

如果你想参与并贡献你的代码，那么你可以按照以下步骤进行：

1. **复刻项目仓库到本地**
    - 访问项目的 [GitHub 仓库页面](https://github.com/CatIsNotFound/CContainerKit)。
    - 点击页面右上角的 `Fork` 按钮，将项目复刻到你自己的 GitHub 账户下。
    - 复刻完成后，在你自己账户下的项目仓库页面，点击 `Code` 按钮，复制仓库的 HTTPS 或 SSH 链接。
    - 打开本地终端，使用 `git clone` 命令将仓库克隆到本地，示例命令如下：
        ```bash
        git clone https://github.com/<你的用户名>/CContainerKit
        ```

2. **创建一个新的分支，用于开发新功能或修复 bug**
    - 进入克隆到本地的项目目录，使用以下命令切换到主分支：
      ```bash
      git checkout feature
      ```
    !!! note
        - 主分支通常是 `feature` 或 `bugfix`，分别用于开发新功能或修复 bug。
        - 你也可以选择其他分支，例如 `hotfix` 或 `debug`，但请确保你选择的分支是用于开发功能或修复 bug 的分支。

    - 拉取最新的代码：
      ```bash
      git pull origin feature
      ```
    - 创建并切换到一个新的分支，分支名建议使用有描述性的名称，例如 `feature/add-login` 或 `bugfix/fix-login-issue`，示例命令如下：
      ```bash
      git checkout -b <新分支名>
      ```

3. **进行代码编写和测试**
    - 使用你喜欢的代码编辑器打开项目，按照需求编写代码。
    - 编写代码时，请遵循项目的代码规范和风格。
    - 编写完成后，在本地运行项目的测试用例，确保新代码没有引入新的错误或问题。如果没有测试用例，也请手动测试新功能或修复的问题。

4. **提交代码并创建一个 Pull Request**
    - 在终端中，使用以下命令将修改的文件添加到暂存区：
      ```bash
      git add .
      ```
    - 使用 `git commit` 命令提交代码，提交信息应简洁明了地描述本次修改的内容，示例命令如下：
      ```bash
      git commit -m "添加登录功能"
      ```
    - 使用 `git push` 命令将本地分支推送到你自己账户下的 GitHub 仓库，示例命令如下：
      ```bash
      git push origin <新分支名>
      ```
    - 回到你自己账户下的 GitHub 仓库页面，点击页面上的 `Compare & pull request` 按钮，填写 Pull Request 的标题和描述，然后点击 `Create pull request` 按钮。

5. **等待项目维护者进行代码审查和合并**
    - 项目维护者会收到你的 Pull Request 请求，并对你的代码进行审查。
    - 审查过程中，维护者可能会提出一些修改意见，你需要根据意见对代码进行修改，然后再次提交。
    - 当代码通过审查后，维护者会将你的代码合并到主项目中。
# 快速上手

-----

本快速上手指南将帮助您快速了解库的安装方法、核心功能及基本使用流程，让您能够在项目中迅速集成并高效使用 CContainerKit。

## 编译安装项目

### 第一步：克隆项目

若已安装 [Git](https://git-scm.com/downloads)，请直接使用下方命令已克隆项目：

```bash
git clone https://github.com/CatIsNotFound/CContainerKit.git
```

克隆完成后，使用 `ls` 命令，你应该能够找到名为 `CContainerKit` 目录。

### 第二步：编译安装

此步骤下，需要根据不同的操作系统和编译器并使用 [CMake](https://cmake.org/download/) 进行不同的操作。

#### Ninja (Linux)

这里非常推荐 Linux 环境下使用 [Ninja](https://ninja-build.org/) 进行编译。*(注：Ninja 需要通过包管理器或通过 [Github](https://github.com/ninja-build/ninja/releases) 下载安装)*

进入 `CContainerKit` 目录，执行以下命令编译项目：

```bash
cd CContainerKit
mkdir build && cd build
cmake .. -G "Ninja" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR> 
ninja install
```

!!! note
    请将 `<INSTALL_DIR>` 替换为您希望安装的目录路径。当执行 cmake 配置命令时，在终端中你可以看到输出的安装路径。通常会找到如下行内容：

    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```

#### GCC (Linux)

进入 `CContainerKit` 目录，执行以下命令编译项目：

```bash
cd CContainerKit
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
make install
```

!!! note
    请将 `<INSTALL_DIR>` 替换为您希望安装的目录路径。当执行 cmake 配置命令时，在终端中你可以看到输出的安装路径。通常会找到如下行内容：
    
    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```

#### MinGW (Windows)

进入 `CContainerKit` 目录，执行以下命令编译项目：

```bash
cd CContainerKit
mkdir build && cd build
cmake.. -G "MinGW Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
mingw32-make install
```

!!! note
    请将 `<INSTALL_DIR>` 替换为您希望安装的目录路径。当执行 cmake 配置命令时，在终端中你可以看到输出的安装路径。通常会找到如下行内容：

    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```

#### MSVC (Windows)

本教程推荐以 [NMake](https://learn.microsoft.com/en-us/cpp/build/reference/nmake-reference?view=msvc-170) 构建工具（一般情况下，最新版本的 MSVC 已内置此构建工具）进行构建。

请在 Windows 下打开 `x64 Native Tools Command Prompt for VS 2022` 终端。

进入 `CContainerKit` 目录，执行以下命令编译项目：

```bash
cd /path/to/CContainerKit
mkdir build && cd build
cmake.. -G "NMake Makefiles" -A x64 -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
nmake install
```

如果使用 [MSBuild](https://learn.microsoft.com/en-us/cpp/build/reference/msbuild-reference-cpp?view=msvc-170) 构建工具，执行以下命令编译项目：

```bash
cd /path/to/CContainerKit
mkdir build && cd build
cmake.. -G "Visual Studio 17 2022" -A x64 -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
cmake --build . --config=Release --target=install
```

!!! note
    请将 `<INSTALL_DIR>` 替换为您希望安装的目录路径。当执行 cmake 配置命令时，在终端中你可以看到输出的安装路径。通常会找到如下行内容：

    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```


## 如何在新建的项目中导入库

本教程非常推荐使用 CMake 来管理项目。具体导入步骤如下：

1. 在自己新建的项目中，创建一个 `CMakeLists.txt` 文件。
2. 在 `CMakeLists.txt` 文件中添加以下内容：

    ```cmake
    cmake_minimum_required(VERSION 3.10)
    project(MyProject)
    set(CMAKE_C_STANDARD 11)

    # You have to set the CMAKE_PREFIX_PATH to the install directory of CContainerKit.
    set(CMAKE_PREFIX_PATH /path/to/CContainerKit/)
    find_package(CContainerKit REQUIRED)

    add_executable(MyProject main.c)

    target_link_libraries(MyProject PRIVATE CContainerKit::CContainerKit)
    ```

    !!! note
        请将 `/path/to/CContainerKit/` 替换为您在编译项目时指定的安装路径。

        请将 `MyProject` 替换为您实际的项目名称。

3. 在 `main.c` 文件中添加以下内容：

    ```c
    #include <CContainerKit.h>

    int main() {
        debug("Hello world!");
        return 0;
    }
    ```

4. 编译并运行项目。

    你将看到输出的内容如下：

    ```
    [Debug] Hello world!
    ```

## 最后

恭喜你！你已经成功地安装并使用 CContainerKit 库。

!!! tip "另请参阅"
    关于 CContainerKit 的更多信息，请参考 [API 文档](../apis/index.md)以及[示例代码](../demos/index.md)。

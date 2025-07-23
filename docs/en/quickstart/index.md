
# Quick Start

-----

This quick start guide will help you quickly understand the library's installation method, core functions, and basic usage, allowing you to quickly integrate and efficiently use CContainerKit in your projects.

## Compile and Install the Project

### Step 1: Clone the Project

If you have [Git](https://git-scm.com/downloads) installed, please use the following command to clone the project:

```bash
git clone https://github.com/CatIsNotFound/CContainerKit.git
```

After cloning is complete, use the `ls` command and you should be able to find a directory named `CContainerKit`.

### Step 2: Compile and Install

In this step, you need to use [CMake](https://cmake.org/download/) for different operations depending on your operating system and compiler.

#### Clang (MacOS/Linux)

On MacOS/Linux, compile the project using [Clang](https://clang.llvm.org/).

??? tip "How to install Clang?"
    **MacOS:**

    **Option 1: Install Xcode Command Line Tools [Recommended]**

    Execute the following command, which will display a graphical window. Follow the prompts to install:

    ```bash
    xcode-select --install
    ```

    **Option 2: Install via package manager**
    
    You can install Clang via [Homebrew](https://brew.sh/).

    ```bash
    brew install clang
    ```

    **Linux:**

    You can install Clang via your package manager (typically using `apt` or `dnf`).

    ```bash
    sudo apt install clang lldb lld make                            // Debian/Ubuntu
    sudo dnf install clang clang-devel lldb lld                     // Fedora/CentOS/RHEL
    sudo pacman -S clang clang-tools-extra                          // Arch Linux/Manjaro
    ```

Enter the `CContainerKit` directory and execute the following commands to compile the project:

```bash
cd CContainerKit
mkdir build && cd build
cmake .. -G "UNIX Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
make install
```

!!! note
    Please replace `<INSTALL_DIR>` with your desired installation directory path. When you execute the cmake configuration command, you can see the output installation path in the terminal. You will usually find a line like this:
    
    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```


#### Ninja (Linux)

It is highly recommended to use [Ninja](https://ninja-build.org/) for compilation in a Linux environment. *(Note: Ninja needs to be installed via package manager or downloaded from [Github](https://github.com/ninja-build/ninja/releases))*

!!! note
    When compiling the project using ninja, make sure a compiler is already installed on your system. See the [GCC (Linux)](#gcc-linux) section below for details.

??? tip "How to install ninja"
    On different Linux distributions, you may need to use different package managers to install Ninja.

    ```bash
    sudo apt install ninja-build -y     // Debian/Ubuntu
    sudo dnf install ninja-build -y     // Fedora/CentOS/RHEL
    sudo pacman -S ninja                // Arch Linux/Manjaro
    ```

Enter the `CContainerKit` directory and execute the following commands to compile the project:

```bash
cd CContainerKit
mkdir build && cd build
cmake .. -G "Ninja" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR> 
ninja install
```

!!! note
    Please replace `<INSTALL_DIR>` with your desired installation directory path. When you execute the cmake configuration command, you can see the output installation path in the terminal. You will usually find a line like this:

    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```

#### GCC (Linux)

On Linux systems, compile the project using [GCC](https://gcc.gnu.org/).

??? tip "How to install GCC?"
    On Linux systems, you can install GCC using the package manager.

    ```bash
    sudo apt install gcc g++ make gdb -y      // Debian/Ubuntu
    sudo dnf install gcc gcc-c++ make gdb -y  // Fedora/CentOS/RHEL
    sudo pacman -S gcc g++ make gdb           // Arch Linux/Manjaro
    ```

Enter the `CContainerKit` directory and execute the following commands to compile the project:

```bash
cd CContainerKit
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
make install
```

!!! note
    Please replace `<INSTALL_DIR>` with your desired installation directory path. When you execute the cmake configuration command, you can see the output installation path in the terminal. You will usually find a line like this:
    
    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```

#### MinGW (Windows)

Enter the `CContainerKit` directory and execute the following commands to compile the project:

```bash
cd CContainerKit
mkdir build && cd build
cmake.. -G "MinGW Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
mingw32-make install
```

!!! note
    Please replace `<INSTALL_DIR>` with your desired installation directory path. When you execute the cmake configuration command, you can see the output installation path in the terminal. You will usually find a line like this:

    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```

#### MSVC (Windows)

This tutorial recommends using the [NMake](https://learn.microsoft.com/en-us/cpp/build/reference/nmake-reference?view=msvc-170) build tool (generally, the latest version of MSVC has this build tool built-in).

Open the `x64 Native Tools Command Prompt for VS 2022` terminal on Windows.

Enter the `CContainerKit` directory and execute the following commands to compile the project:

```bash
cd /path/to/CContainerKit
mkdir build && cd build
cmake.. -G "NMake Makefiles" -A x64 -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
nmake install
```

If using the [MSBuild](https://learn.microsoft.com/en-us/cpp/build/reference/msbuild-reference-cpp?view=msvc-170) build tool, execute the following commands to compile the project:

```bash
cd /path/to/CContainerKit
mkdir build && cd build
cmake.. -G "Visual Studio 17 2022" -A x64 -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
cmake --build . --config=Release --target=install
```

!!! note
    Please replace `<INSTALL_DIR>` with your desired installation directory path. When you execute the cmake configuration command, you can see the output installation path in the terminal. You will usually find a line like this:

    ```
    -- CMAKE_INSTALL_PREFIX: <INSTALL_DIR>
    ```


## How to Import the Library in a New Project

This tutorial highly recommends using CMake to manage your project. The specific import steps are as follows:

1. In your newly created project, create a `CMakeLists.txt` file.
2. Add the following content to the `CMakeLists.txt` file:

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
        Please replace `/path/to/CContainerKit/` with the installation path you specified when compiling the project.

        Please replace `MyProject` with your actual project name.

3. Add the following content to the `main.c` file:

    ```c
    #include <CContainerKit.h>

    int main() {
        debug("Hello world!");
        return 0;
    }
    ```

4. Compile and run the project.

    You will see the output as follows:

    ```
    [Debug] Hello world!
    ```


## Conclusion

Congratulations! You have successfully installed and used the CContainerKit library.

!!! tip "See also"
    For more information about CContainerKit, please refer to the [API Documentation](../apis/index.md) and [Code Demos](../demos/index.md).
        
--8<-- "includes/en/abbreviations.md"

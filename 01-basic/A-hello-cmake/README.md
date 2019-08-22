展示一个非常基础的 hello world 示例

<!--more-->

# mac 安装 cmake

[下载地址](https://cmake.org/download/)

安装完 dmg 文件后，启动它， 找到菜单 Tools > How to install for command line use, 按照弹出框提示增加 cmake 的命令行工具

如： `sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install`


安装 tree 命令 `$ brew install tree`

# 介绍

教程中的文件结构如下:

```
A-hello-cmake$ tree
├── CMakeLists.txt
├── README.md
└── main.cpp

```

- CMakeLists.txt - 包含您希望运行的 CMake 命令
- main.cpp - 一个简单的 “Hello World” cpp文件

# 概念

### CMakeLists.txt

CMakeLists.txt 是存储所有 CMake 命令的文件。当 cmake 在该文件夹中运行时，它将查找此文件，如果它不存在，cmake 将退出并显示错误。

### 最低的 CMake 版本

使用 CMake 创建项目时，您可以指定支持的最小 CMake 版本。

```cmake
cmake_minimum_required(VERSION 3.5)
```


### 项目

CMake 构建时可以包含项目名称，以便在使用多个项目时更容易引用某些变量。

```cmake
project (hello_cmake)
```

### 创建可执行文件

add_executable() 命令指定应从指定的源文件构建可执行文件，在此示例中为 `main.cpp`。
add_executable() 函数的第一个参数是要构建的可执行文件的名称，第二个参数是要编译的源文件的列表。

```cmake
add_executable(hello_cmake main.cpp)
```

>  一些人使用简写的项目名称，它和可执行文件名相同。像如下所示 CMakeLists.txt
```cmake
    cmake_minimum_required(VERSION 2.6)
    project (hello_cmake)
    add_executable(${PROJECT_NAME} main.cpp)
```
>在此示例中，project() 函数创建变量 ${PROJECT_NAME}，并赋予它 hello_cmake 值。
然后可以将其传递给 add_executable() 函数，以输出 'hello_cmake' 可执行文件。


### 二进制文件夹

运行 cmake 命令的根文件夹或顶级文件夹我们称为 CMAKE_BINARY_DIR，它是所有二进制文件的根文件夹。
CMake 支持在原地和源码文件外构建并生成二进制文件。

#### 就地构建

就地构建是在与源代码相同的目录结构中生成所有临时构建文件。 这意味着所有 Makefile 和目标文件都穿插在您的普通代码中。
要创建就地构建目标，请在根目录中运行 cmake 命令。 例如：

```bash
A-hello-cmake$ cmake .
-- The C compiler identification is AppleClang 10.0.0.10001145
-- The CXX compiler identification is AppleClang 10.0.0.10001145
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/max/working/clang/cmake-examples/01-basic/A-hello-cmake
```

```bash
A-hello-cmake$ tree
.
├── CMakeCache.txt
├── CMakeFiles
│   ├── 3.15.2
│   │   ├── CMakeCCompiler.cmake
│   │   ├── CMakeCXXCompiler.cmake
│   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   ├── CMakeSystem.cmake
│   │   ├── CompilerIdC
│   │   │   ├── CMakeCCompilerId.c
│   │   │   ├── a.out
│   │   │   └── tmp
│   │   └── CompilerIdCXX
│   │       ├── CMakeCXXCompilerId.cpp
│   │       ├── a.out
│   │       └── tmp
│   ├── CMakeDirectoryInformation.cmake
│   ├── CMakeOutput.log
│   ├── CMakeTmp
│   ├── Makefile.cmake
│   ├── Makefile2
│   ├── TargetDirectories.txt
│   ├── cmake.check_cache
│   ├── hello_cmake.dir
│   │   ├── DependInfo.cmake
│   │   ├── build.make
│   │   ├── cmake_clean.cmake
│   │   ├── depend.make
│   │   ├── flags.make
│   │   ├── link.txt
│   │   └── progress.make
│   └── progress.marks
├── CMakeLists.txt
├── Makefile
├── README.md
├── cmake_install.cmake
└── main.cpp
```

#### 外部构建

源外构建允许您可以创建在文件系统上的任何位置的单个构建文件夹中。所有临时构建和目标文件都位于此目录中，以保持源码树的整洁。
要创建源外构建，请在顶层文件夹中运行 cmake 命令，并加上 -B 参数。
如果要从头开始重新创建 cmake 环境，请使用源外构建，只需删除构建目录，然后重新运行 cmake 命令。
例如：

```bash
A-hello-cmake$ mkdir build

A-hello-cmake$ cmake . -Bbuild
-- The C compiler identification is AppleClang 10.0.0.10001145
-- The CXX compiler identification is AppleClang 10.0.0.10001145
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/max/working/clang/cmake-examples/01-basic/A-hello-cmake/build

A-hello-cmake$ tree
.
├── CMakeLists.txt
├── README.md
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   │   ├── 3.15.2
│   │   │   ├── CMakeCCompiler.cmake
│   │   │   ├── CMakeCXXCompiler.cmake
│   │   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   │   ├── CMakeSystem.cmake
│   │   │   ├── CompilerIdC
│   │   │   │   ├── CMakeCCompilerId.c
│   │   │   │   ├── a.out
│   │   │   │   └── tmp
│   │   │   └── CompilerIdCXX
│   │   │       ├── CMakeCXXCompilerId.cpp
│   │   │       ├── a.out
│   │   │       └── tmp
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── CMakeOutput.log
│   │   ├── CMakeTmp
│   │   ├── Makefile.cmake
│   │   ├── Makefile2
│   │   ├── TargetDirectories.txt
│   │   ├── cmake.check_cache
│   │   ├── hello_cmake.dir
│   │   │   ├── DependInfo.cmake
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   └── progress.make
│   │   └── progress.marks
│   ├── Makefile
│   └── cmake_install.cmake
└── main.cpp


```

本教程中的所有示例都将使用源外构建。


# 构建示例

以下是构建此示例的输出。

```bash

$ cd build

$ make
Scanning dependencies of target hello_cmake
[ 50%] Building CXX object CMakeFiles/hello_cmake.dir/main.cpp.o
[100%] Linking CXX executable hello_cmake
[100%] Built target hello_cmake


$ ./hello_cmake
Hello CMake!
----

```
# 介绍

这里演示一个 hello world 示例， 它使用不同 source 文件和 include 文件

本教程中包含如下文件:

```
B-hello-headers$ tree
.
├── CMakeLists.txt
├── include
│   └── Hello.h
└── src
    ├── Hello.cpp
    └── main.cpp
```

- link:CMakeLists.txt[CMakeLists.txt] - 包含你要运行的 CMake 命令.
- link:include/Hello.h[include/Hello.h] - include 包含的头文件.
- link:src/Hello.cpp[src/Hello.cpp] - 要编译的源文件.
- link:src/main.cpp[src/main.cpp] - main 源文件.

### 源代码如下

CMakeLists.txt：

```cmake

# Set the minimum version of CMake that can be used
# To find the cmake version run
# $ cmake --version
cmake_minimum_required(VERSION 3.5)

# Set the project name
project (hello_headers)

# Create a sources variable with a link to all cpp files to compile
set(SOURCES
    src/Hello.cpp
    src/main.cpp
)

# Add an executable with the above sources
add_executable(hello_headers ${SOURCES})

# Set the directories that should be included in the build command for this target
# when running g++ these will be included as -I/directory/path/
target_include_directories(hello_headers
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include
)


```


Hello.cpp

```c++
#include <iostream>

#include "Hello.h"

void Hello::print()
{
    std::cout << "Hello Headers!" << std::endl;
}



```


main.cpp

```c++

#include "Hello.h"

int main(int argc, char *argv[])
{
    Hello hi;
    hi.print();
    return 0;
}


```

Hello.h

```c++

#ifndef __HELLO_H__
#define __HELLO_H__

class Hello
{
public:
    void print();
};

#endif


```


# 概念

## 目录路径

CMake 语法定义了一些 [变量](https://cmake.org/Wiki/CMake_Useful_Variables),这些变量可以帮助我们在项目中或者源代码树里找到我们想要的目录，其中一些如下：


变量              | 说明
-----------------|--------------------
CMAKE_SOURCE_DIR | 根源代码目录
CMAKE_CURRENT_SOURCE_DIR | 当使用子项目和目录时，它表示当前源代码目录
PROJECT_SOURCE_DIR | 当前 cmake 项目的源代码目录
CMAKE_BINARY_DIR | 根 二进制/构建 目录，在这个目录下面你可以运行 cmake 命令
CMAKE_CURRENT_BINARY_DIR | 当前所在的构建目录
PROJECT_BINARY_DIR | 当前项目的构建目录

## 源代码文件变量

创建包含源文件的变量, 这样您会更清楚有哪些源文件，并可以轻松把它们添加到多个命令中，例如， `add_executable()` 函数。

```cmake
# Create a sources variable with a link to all cpp files to compile
set(SOURCES
    src/Hello.cpp
    src/main.cpp
add_executable(${PROJECT_NAME} ${SOURCES})
```

**在 `SOURCES` 变量中设置特定文件名的替代方法是使用 GLOB 命令, 使用通配符模式匹配查找文件。**

```cmake
file(GLOB SOURCES "src/*.cpp")
```

> 对于现代的 CMake，不建议将变量用于源文件。 相反，通常采用在 add_xxx 函数中直接声明源文件的方式。需要注意的是，在你添加一个源文件的时候，使用 glob 命令并不总是能得到正确的结果，

## 包含的目录

如果有不同的 include 文件夹，你可以使用 `target_include_directories()` [函数](https://cmake.org/cmake/help/v3.0/command/target_include_directories.html)
让编译器识别它们, 在编译此目标时，它会使用 `-I` 标志将这些目录添加到编译器，例如： `-I/directory/path`

```cmake
target_include_directories(target
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include
)
```

`PRIVATE` 标识符指定包含的范围。 这对于库来说很重要，我在下一个示例中会进行说明。 有关该功能的更多详细信息，请访问[以下链接](https：//cmake.org/cmake/help/v3.0/command/target_include_directories.html)

# 构建这个示例

## 标准输出

构建此示例的标准输出如下所示。

```bash

➜  B-hello-headers git:(master) ✗ mkdir build
➜  B-hello-headers git:(master) ✗ cmake . -Bbuild
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
-- Build files have been written to: /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build

➜  B-hello-headers git:(master) ✗ cd build
➜  build git:(master) ✗ make VERBOSE=1
/Applications/CMake.app/Contents/bin/cmake -S/Users/max/working/clang/cmake-examples/01-basic/B-hello-headers -B/Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build --check-build-system CMakeFiles/Makefile.cmake 0
/Applications/CMake.app/Contents/bin/cmake -E cmake_progress_start /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build/CMakeFiles /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build/CMakeFiles/progress.marks
/Applications/Xcode.app/Contents/Developer/usr/bin/make -f CMakeFiles/Makefile2 all
/Applications/Xcode.app/Contents/Developer/usr/bin/make -f CMakeFiles/hello_headers.dir/build.make CMakeFiles/hello_headers.dir/depend
cd /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build && /Applications/CMake.app/Contents/bin/cmake -E cmake_depends "Unix Makefiles" /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build/CMakeFiles/hello_headers.dir/DependInfo.cmake --color=
Dependee "/Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build/CMakeFiles/hello_headers.dir/DependInfo.cmake" is newer than depender "/Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build/CMakeFiles/hello_headers.dir/depend.internal".
Dependee "/Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build/CMakeFiles/hello_headers.dir/depend.internal".
Scanning dependencies of target hello_headers
/Applications/Xcode.app/Contents/Developer/usr/bin/make -f CMakeFiles/hello_headers.dir/build.make CMakeFiles/hello_headers.dir/build
[ 33%] Building CXX object CMakeFiles/hello_headers.dir/src/Hello.cpp.o
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++   -I/Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/include  -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.14.sdk   -o CMakeFiles/hello_headers.dir/src/Hello.cpp.o -c /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/src/Hello.cpp
[ 66%] Building CXX object CMakeFiles/hello_headers.dir/src/main.cpp.o
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++   -I/Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/include  -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.14.sdk   -o CMakeFiles/hello_headers.dir/src/main.cpp.o -c /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/src/main.cpp
[100%] Linking CXX executable hello_headers
/Applications/CMake.app/Contents/bin/cmake -E cmake_link_script CMakeFiles/hello_headers.dir/link.txt --verbose=1
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++   -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.14.sdk -Wl,-search_paths_first -Wl,-headerpad_max_install_names  CMakeFiles/hello_headers.dir/src/Hello.cpp.o CMakeFiles/hello_headers.dir/src/main.cpp.o  -o hello_headers
[100%] Built target hello_headers
/Applications/CMake.app/Contents/bin/cmake -E cmake_progress_start /Users/max/working/clang/cmake-examples/01-basic/B-hello-headers/build/CMakeFiles 0
➜  build git:(master) ✗ ./hello_headers
Hello Headers!

```

## 更多的输出

在上一次的示例中，运行 make 命令时仅会输出显示构建的状态。如果想查看完整输出以进行调试，你可以在运行 make 命令时添加 `VERBOSE=1` 标志。
上面的输出信息显示，include 目录已被添加到 c++ 编译器命令中。



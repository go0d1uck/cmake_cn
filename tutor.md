# CMake 教程

## 简介

这份 CMake 教程手把手教你用 Cmake 解决一些常见的构建问题。观察不同的主题融合在一个项目中是很有帮助的，
这份教程文件和源代码可以在源代码中的 Help/guide/tutorial 目录找到，每一步都有对应的子目录，其中包含了源代码，可以以这些子目录作为起点。教程样例是连续的，因此每一步都是上一步的完整解决方案。

## 最初起点(Step 1)

大部分的项目都是从源文件中构建可执行文件，对于一个简单的项目，有三行代码是必备的，这将作为我们的教程的起点，现在目录 Step1目录下创建一个
CMakeLists.txt 文件
如下所示

```cmake
cmake_minimum_required(VERSION 3.10)

project(Tutorial)

add_executable(Tutorial tutorial.cxx)
```

注意到这个例子所使用的命令都是小写，但实际上在 CMakeLists.txt 中是不区分大小写的。
在 Step1 目录下的 tutorial.cxx 源代码文件，可以用来计算一个数的平方根。

### 添加版本号和配置头文件

我们将要加上的第一个特性是给我们的项目和可执行程序加上版本号。虽然我们可以在源代码中加入版本号，但是使用 CMakeLists.txt 可以提供更多的灵活性。

首先修改 CMakeLists.txt ，使用 project() 命令去设置项目名字和版本代码

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial VERSION 1.0)
```

然后配置一个头文件将版本号传给源代码。

```cmake
configure_file(config.in config.h)
```

由于配置文件会被写入到二进制树，我们必须将该路径添加进头文件列表中，使用如下一行在 CMakeLists.txt 文件的末尾

```cmake
target_include_directories(Tutorial PUBLIC "${PROJECT_BINARY_DIR}")
```

使用你最爱的编辑器编辑 config.in 文件，让其包含如下代码：

```cpp
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```

### 指明 C++ 标准

下一步，让我们使用一些 C++11 标准,使用 `std::stod` 代替 `atof` ，于此同时移除 `#include <cstdlib>`



# CMake介绍

CMake 是个一个开源的**跨平台自动化建构系统**，用来管理软件建置的程序，并**不依赖于某特定编译器**，并可支持多层目录、多个应用程序与多个函数库。

**CMake 本身不是构建工具，而是生成构建系统的工具**，它生成的构建系统可以使用不同的编译器和工具链。

CMake 通过使用简单的配置文件 **CMakeLists.txt**，自动生成不同平台的构建文件（如 Makefile、Ninja 构建文件、Visual Studio 工程文件等），简化了项目的编译和构建过程。

>  在编译C++项目时，会用到Makefile文件，但是编写Makefile过于繁琐。为了使这一过程方便快捷，诞生了CMake这一工具，可以说，CMake就是为了方便生成Makefile而产生的。

## 基本工作流程

1. 编写**CMakeLists.txt**文件：定义项目的构建规则和依赖关系
2. 生成**构建文件**：使用CMake生成适合当前平台的构建系统文件（例如 Makefile、Visual Studio 工程文件）
3. **执行构建**：使用生成的构建系统文件（如make、ninja、msbuild）来编译项目

<img src="pic/Single_Source_Build-cmake.png" alt="img" style="zoom: 40%;" />



## CMake安装

Linux（Ubuntu）

~~~shell
sudo apt-get install cmake
cmake --version
~~~



## CMakeLists.txt

CMakeLists.txt 是 CMake 的配置文件，用于定义项目的构建规则、依赖关系、编译选项等。

每个 CMake 项目通常都有一个或多个 CMakeLists.txt 文件。

CMakeLists.txt 文件使用一系列的 CMake 指令来描述构建过程。

### 常见指令

指定CMake的最低版本要求

```shell
cmake_minimum_required(VERSION <version>)
# 例如
cmake_minimum_required(VERSION 3.10)
```

定义项目的名称和使用的语言

~~~shell
project(<project_name> [<language>...])
# 例如
project(MyProject CXX)
~~~

指定要生成的可执行文件和其源文件

~~~shell
add_executable(<target> <source_files>...)
# 例如
add_executable(MyLibrary STATIC library.cpp)
~~~

创建一个库（静态库或动态库）及其源文件

~~~shell
add_library(<target> <source_files>...)
~~~

链接目标文件与其他库

~~~shell
target_link_libraries(<target> <libraries>...)
~~~

添加头文件搜索路径

~~~shell
include_directories(<dirs>...)
~~~

设置变量的值

~~~shell
set(<variable> <value>...)
~~~



## CMake 构建流程

1. **创建构建目录**：

   - 推荐out-of-source方式，即将构建文件放在源代码目录之外的独立目录中

   - 创建构建目录，例如在项目根目录下创建 build

     ~~~shell
     mkdir build
     cd build
     ~~~

2. **使用 CMake 生成构建文件**：

   - 在构建目录（build）中运行CMake，生成构建系统文件（如Makefile、Ninja构建文件）

     ~~~shell
     cmake ..
     ~~~

3. **编译和构建**：

   - 使用生成的构建文件（Makefile）进行编译和构建

   - 如果使用 Makefile，可以运行 make 命令来编译和构建（不同构件系统使用不同的命令）

     ~~~shell
     make
     ~~~

   - 如果要构建特定的目标，可以指定目标名称

     ~~~shell
     make MyExecutable
     ~~~

4. **清理构建文件**：

   - 构建过程中生成的中间文件和目标文件可以通过清理操作删除

     ~~~shell
     make clean
     ~~~

5. **重新配置和构建**：

   - 如果修改了 CMakeLists.txt 文件或项目设置，可能需要重新配置和构建项目

   - 重新运行CMake配置

     ~~~shell
     cmake ..
     ~~~

   - 重新编译

     ~~~shell
     make
     ~~~

     

## 示例

假设有项目结构如下：

```shell
MyProject/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   └── mylib.cpp
└── include/
    └── mylib.h
```

- `main.cpp`：主程序源文件。
- `mylib.cpp`：库源文件。
- `mylib.h`：库头文件。
- `CMakeLists.txt`：CMake 配置文件

#### 创建CMakeLists.txt：

~~~shell
cmake_minimum_required(VERSION 3.10)   # 指定最低 CMake 版本

project(MyProject VERSION 1.0)          # 定义项目名称和版本

# 设置 C++ 标准为 C++11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 添加头文件搜索路径
include_directories(${PROJECT_SOURCE_DIR}/include)

# 添加源文件
add_library(MyLib src/mylib.cpp) # 创建一个名为MyLib的库 源文件是mylib.cpp
add_executable(MyExecutable src/main.cpp) # 创建一个名为MyExecutable可执行文件 源文件是main.cpp

# 链接库到可执行文件 将 MyLib 库链接到 MyExecutable 可执行文件
target_link_libraries(MyExecutable MyLib) 
~~~

#### 创建构建目录

打开终端，进入 MyProject 目录，然后创建构建目录：

~~~shell
mkdir build
cd build
~~~

#### 配置项目

在构建目录（build）中运行 CMake 配置命令，生成适合平台的构建系统文件（如 Makefile）：

~~~shell
cmake ..
~~~

#### 编译项目

使用生成的构建系统文件编译项目。（如果是Makefile，使用make命令进行编译）

~~~shell
make
~~~

编译项目并生成可执行文件 `MyExecutable`

#### 运行可执行文件

编译完成后，在构建目录（build）中，运行生成的可执行文件 `MyExecutable`

~~~shell
./MyExecutable
~~~

#### 清理构建文件

清理构建文件以删除生成的中间文件和目标文件。

如果在 CMakeLists.txt 中定义了清理规则，可以使用 **make clean** 命令：

~~~shell
make clean
~~~


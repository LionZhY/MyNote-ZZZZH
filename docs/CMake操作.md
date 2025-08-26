

# CMake介绍

CMake 是个一个开源的**跨平台自动化建构系统**，用来管理软件建置的程序，并**不依赖于某特定编译器**，并可支持多层目录、多个应用程序与多个函数库。

**CMake 本身不是构建工具，而是生成构建系统的工具**，它生成的构建系统可以使用不同的编译器和工具链。

CMake 通过使用简单的配置文件 **CMakeLists.txt**，自动**生成不同平台的构建文件**（如 Makefile、Ninja 构建文件、Visual Studio 工程文件等），简化了项目的编译和构建过程。

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

     

# CMakeLists.txt

CMakeLists.txt 是 CMake 的配置文件，用于定义项目的构建规则、依赖关系、编译选项等。

每个 CMake 项目通常都有一个或多个 CMakeLists.txt 文件。

CMakeLists.txt 文件使用一系列的 CMake 指令来描述构建过程。

## 常见指令

### 指定CMake的最低版本

```shell
cmake_minimum_required(VERSION <version>)

# 例如
cmake_minimum_required(VERSION 3.10)
```



### 设置项目名称和使用语言

~~~shell
project(<project_name> [<language>...])

# 例如
project(MyProject CXX)
# 或者不写使用的语言
project(MyProject)
~~~



### 生成可执行文件

~~~shell
add_executable(<target> <source_files>...)

# 例如
add_executable(demo demo.cpp) 
~~~

在 Linux 下生成的是：demo

在 Windows 下生成的是：demo.exe



### 生成库

~~~shell
add_library(<target> <source_files>...)

# 例如
add_library(common STATIC util.cpp) # 生成静态库
add_library(common SHARED util.cpp) # 生成动态库或共享库
~~~

add_library 默认生成是静态库，通过以上命令生成文件名字，

- 在 Linux 下是：
  libcommon.a
  libcommon.so
- 在 Windows 下是：
  common.lib
  common.dll

**明确指定包含哪些源文件**

~~~shell
add_library(demo demo.cpp test.cpp util.cpp)
~~~

**搜索所有的cpp文件**

~~~shell
# 发现一个目录下所有的源代码文件并将列表存储在一个变量中
aux_source_directory(. SRC_LIST) # 搜索当前目录下的所有.cpp文件
add_library(demo ${SRC_LIST})
~~~



### 链接target与其他库

~~~shell
target_link_libraries(<target> <libraries>...)

# 例如                 目标文件        目标库需要连接的库
target_link_libraries(MyExecutable MyLibrary)
~~~

在 Windows 下，系统会根据链接库目录，搜索 `xxx.lib` 文件，Linux 下会搜索 `xxx.so` 或者 `xxx.a` 文件，如果都存在会优先链接动态库（so 后缀）。

**指定链接动态库或静态库**

~~~shell
target_link_libraries(demo libface.a)  # 链接libface.a
target_link_libraries(demo libface.so) # 链接libface.so
~~~

**指定全路径**

~~~shell
target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a)
target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.so)
~~~

**指定链接多个库**

~~~shell
target_link_libraries(demo
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a
    boost_system.a
    boost_thread
    pthread)
~~~



### 添加头文件搜索路径

~~~shell
include_directories(<dirs>...)

# 例如
include_directories(${PROJECT_SOURCE_DIR}/include)
~~~



### 查找库文件

`find_library(VAR name path)` 查找到指定的预编译库，并将它的路径存储在变量中。
默认的搜索路径为 cmake 包含的系统库，因此如果是 NDK 的公共库只需要指定库的 name 即可。

~~~shell
find_library( # Sets the name of the path variable.
              log-lib
 
              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )
~~~

类似的命令还有 find_file()、find_path()、find_program()、find_package()。



### 使用第三方库

假设你想在项目中使用 Boost 库，CMakeLists.txt 文件可能如下所示：

~~~shell
cmake_minimum_required(VERSION 3.10)
project(MyProject CXX)

# 查找 Boost 库
find_package(Boost REQUIRED)

# 添加源文件
add_executable(MyExecutable main.cpp)

# 链接 Boost 库
target_link_libraries(MyExecutable Boost::Boost)
~~~



### 设置变量

~~~shell
set(<variable> <value>...)

# 例如
set(SRC_LIST main.cpp test.cpp)
add_executable(demo ${SRC_LIST})
~~~

常见：

~~~shell
# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
~~~



### 包含其它 cmake 文件

~~~shell
include(./common.cmake) # 指定包含文件的全路径
include(def) # 在搜索路径中搜索def.cmake文件
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake) # 设置include的搜索路径
~~~



### 为target设置头文件搜索路径

~~~shell
target_include_directories(<target>
    [SYSTEM]                  # 可选，将路径作为“系统头”，编译时抑制警告
    <scope>                   # PUBLIC | PRIVATE | INTERFACE
    <directory> [<directory>…]
)
~~~

为**特定目标**（可执行文件或库）设置头文件搜索路径的命令。

相比全局的 `include_directories`，它能够更精准地控制哪些目录对哪些目标可见，以及这些目录对使用该目标的下游目标意味着什么。

- `<target>`：目标名称（通过 `add_library` 或 `add_executable` 定义）。
- `<scope>`：包含目录对不同使用场景的可见性控制。
  - **PRIVATE**：仅对当前 `<target>` 的源文件生效，不会传递给依赖它的其他目标。
  - **PUBLIC**：既对当前 `<target>` 生效，也会传递给链接或依赖该目标的上游目标。
  - **INTERFACE**：仅对依赖或链接该 `<target>` 的上游目标生效，当前目标自身不使用这些目录。
- `SYSTEM`（可选）：将目录标记为“系统头路径”，编译器在这些目录中的头文件警告会被抑制。



**include_directories() 和 target_include_directories()**

在 CMake 中，include_directories() 和 target_include_directories() 都用于**指定头文件的搜索路径**，但它们的作用范围和使用方式有显著区别。

**相同点：**

- 两者都用于添加头文件的搜索路径，编译器会在这些路径中查找 #include 指令中指定的头文件。
- 两者都支持绝对路径和相对路径，相对路径是相对于当前 CMakeLists.txt 文件所在的目录。
- 两者都可以用于指定公共头文件路径（PUBLIC）、私有头文件路径（PRIVATE）或接口头文件路径（INTERFACE）。

**区别：**

| 特性                | `include_directories()`                  | `target_include_directories()`                               |
| :------------------ | :--------------------------------------- | :----------------------------------------------------------- |
| **作用范围**        | 全局作用域，影响所有目标（target）。     | 仅作用于指定的目标（target）。                               |
| **推荐使用场景**    | 适用于简单的项目或旧版 CMake 项目。      | 适用于现代 CMake 项目，推荐优先使用。                        |
| **目标关联性**      | 不直接关联到特定目标，可能影响所有目标。 | 显式关联到特定目标，避免污染其他目标。                       |
| **可维护性**        | 较差，容易导致全局路径污染。             | 较好，路径与目标绑定，逻辑清晰。                             |
| **作用域控制**      | 无法精确控制路径的作用范围。             | 可以通过 `PUBLIC`、`PRIVATE`、`INTERFACE` 精确控制路径的作用范围。 |
| **现代 CMake 推荐** | 不推荐使用，除非有特殊需求。             | 推荐使用，符合现代 CMake 的最佳实践。                        |







## 常用变量

预定义变量：

- `PROJECT_SOURCE_DIR`：工程的根目录
- `PROJECT_BINARY_DIR`：运行 cmake 命令的目录，通常是 `$ {PROJECT_SOURCE_DIR}/build`
- `PROJECT_NAME`：返回通过 `project` 命令定义的项目名称
- `CMAKE_CURRENT_SOURCE_DIR`：当前处理的 CMakeLists.txt 所在的路径
- `CMAKE_CURRENT_BINARY_DIR`：target 编译目录
- `CMAKE_CURRENT_LIST_DIR`：CMakeLists.txt 的完整路径
- `CMAKE_CURRENT_LIST_LINE`：当前所在的行
- `CMAKE_MODULE_PATH`：定义自己的 cmake 模块所在的路径，`SET(CMAKE_MODULE_PATH {PROJECT_SOURCE_DIR}/cmake)`，然后可以用INCLUDE命令来调用自己的模块
- `EXECUTABLE_OUTPUT_PATH`：重新定义目标二进制可执行文件的存放位置
- `LIBRARY_OUTPUT_PATH`：重新定义目标链接库文件的存放位置





## 根目录 CMakeLists.txt

设置 CMake 最低版本和项目名称

~~~shell
# 设置cmake的最低版本
cmake_minimum_required(VERSION 3.22)
# 项目名称
project(MyProject) 
~~~

配置编译参数（C++标准、编译器选项等）

~~~shell
# 全局编译配置
set(CMAKE_CXX_STANDARD 20) # 使用C++20标准
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_BUILD_TYPE "Debug") # 设置构建类型为 Debug，可以进行gdb调试
~~~

设置库和可执行文件输出路径

~~~shell
# 输出路径
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin) # 可执行文件输出路径  ${项目根目录}/bin
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)    # 库文件输出路径 ${项目根目录}/lib
~~~

设置 include 路径、链接路径

~~~shell
# 包含头文件路径
include_directories(${PROJECT_SOURCE_DIR}/src/include)
# 设置库文件搜索路径 （相当于 g++ 中的 -L）
link_directories(${PROJECT_SOURCE_DIR}/lib)
~~~

添加子目录（add_subdirectory）

~~~shell
# 添加子目录
add_subdirectory(src)
add_subdirectory(tests)
~~~





## 子目录 CMakeLists.txt

指定该子模块的源码文件

创建目标（库/可执行程序）

链接依赖库（如 protobuf、muduo、rpc_lib 等）





# 示例

## 目录结构

```shell
MyProject/
├── CMakeLists.txt
├── src/
│   ├── main.cpp # 主程序源文件
│   ├── lib/
│   │   ├── module1.cpp
│   │   ├── module2.cpp
│   ├── include/
│       └── mylib.h 
└── tests/
    ├── test_main.cpp
    └── CMakeLists.txt
```



## 创建CMakeLists.txt

### 项目根目录下

~~~shell
cmake_minimum_required(VERSION 3.10)   # 指定最低 CMake 版本
project(MyProject VERSION 1.0)         # 定义项目名称和版本

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 包含头文件路径
include_directories(${PROJECT_SOURCE_DIR}/src/include)

# 添加子目录
add_subdirectory(src)
add_subdirectory(tests)
~~~

### src 目录下

~~~shell
# 创建库目标 MyLib
add_library(MyLib STATIC
    lib/module1.cpp
    lib/module2.cpp
)

# 指定库 MyLib 的头文件
target_include_directories(MyLib PUBLIC ${CMAKE_SOURCE_DIR}/src/include)
 
# 创建可执行文件目标 MyExecutable
add_executable(MyExecutable main.cpp)

# 链接库到可执行文件 MyExecutable -- MyLib
target_link_libraries(MyExecutable PRIVATE MyLib) 
~~~

### tests 目录下

~~~shell
# 查找 GTest 包
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

# 创建测试目标可执行文件 TestMyLib
add_executable(TestMyLib test_main.cpp)

# 链接 MyLib 库和 GTest 到 TestMyLib
target_link_libraries(TestMyLib PRIVATE MyLib ${GTEST_LIBRARIES})
~~~





## 创建构建目录

打开终端，进入 MyProject 目录，然后创建构建目录：

~~~shell
mkdir build
cd build
~~~

## 配置项目

在构建目录（build）中运行 CMake 配置命令，生成适合平台的构建系统文件（如 Makefile）：

~~~shell
cmake ..
~~~

## 编译项目

使用生成的构建系统文件编译项目。（如果是Makefile，使用make命令进行编译）

~~~shell
make
~~~

编译项目并生成可执行文件 `MyExecutable`

## 运行可执行文件

编译完成后，在构建目录（build）中，运行生成的可执行文件 `MyExecutable`

~~~shell
./MyExecutable
./TestMyLib
~~~

## 清理构建文件

清理构建文件以删除生成的中间文件和目标文件。

如果在 CMakeLists.txt 中定义了清理规则，可以使用 **make clean** 命令：

~~~shell
make clean
~~~


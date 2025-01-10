# CMake 基础语法笔记

## 1. CMake 最低版本要求
```cmake
cmake_minimum_required(VERSION 3.15)
```

- 用于指定 CMake 的最低版本要求，确保兼容性。
## 2. 项目名称

```cmake
project(hello)
```

- 用于定义项目名称。
- 可以包含更多信息：
```cmake
project(hello
    VERSION 1.0                  # 项目版本号
    DESCRIPTION "A simple project with cmake" # 描述
    LANGUAGES CXX                # 使用的语言
)
```
## 3. 添加可执行文件

```cmake
add_executable(hello main.cpp add.cpp)
```

- 定义一个名为 hello 的可执行文件。
- main.cpp 和 add.cpp 为项目的源文件，可以指定多个，用空格隔开。
**搜索源文件：**

```cmake
file(GLOB SRC_LIST ${CMAKE_SOURCE_DIR}/Src/*.cpp)
```

- 自动搜索 Src 目录下的所有 .cpp 文件。
**包含头文件：**

```cmake
include_directories(${CMAKE_SOURCE_DIR}/Inc)
```

- 添加头文件所在目录。
## 4. 添加库文件

### 生成静态库或动态库

```cmake
add_library(hello_lib STATIC ${SRC_LIST}) # 生成静态库
#add_library(hello_lib SHARED ${SRC_LIST}) # 生成动态库
```

- 静态库文件：Windows: hello_lib.libLinux: libhello_lib.a
- 动态库文件：Windows: hello_lib.dllLinux: libhello_lib.so
**完整示例：**

```cmake
cmake_minimum_required(VERSION 3.15)
project(lib_demo)
file(GLOB SRC_LIST ${CMAKE_SOURCE_DIR}/Src/*.cpp)
include_directories(${CMAKE_SOURCE_DIR}/Inc)
add_library(lib_demo STATIC ${SRC_LIST})
#add_library(lib_demo SHARED ${SRC_LIST})
```

### 链接库

#### 静态库

```cmake
link_libraries(lib_demo)
```

#### 动态库

```cmake
target_link_libraries(  hello lib_demo)
```

- **权限说明：**PRIVATE: 仅目标文件可用。PUBLIC: 传递性权限，其他目标也可使用。INTERFACE: 仅其他目标可用。
#### 设置库路径

```cmake
link_directories(${CMAKE_SOURCE_DIR}/Lib)
target_link_directories(
      ${CMAKE_SOURCE_DIR}/Lib
)
```

## 5. 调试信息

使用 `message` 打印调试信息：

```cmake
message(STATUS "hello world")
```

- 参数说明：无参数：重要消息。STATUS: 重要消息。WARNING: 警告。SEND_ERROR: 错误，生成过程被终止。FATAL_ERROR: 严重错误，生成过程被终止。
## 6. 字符串操作

### 数据拼接

```cmake
set(HELLO "hello" "world")         # HELLO = "hello world"
set(HELLO ${HELLO} "world")        # HELLO = "hello world world"
list(APPEND HELLO "world")         # HELLO = "hello world world"
```

- list(APPEND) 是追加，list(PREPEND) 是前插。
### 数据移除

```cmake
list(REMOVE_ITEM HELLO "world")   # 从列表中移除指定值
```

## 7. 宏定义

```cmake
add_definitions(-DHELLO)
```

- 添加编译器宏定义 HELLO。
## 8. 静态库和动态库的区别

- **静态库**: 编译时链接到可执行文件，加载速度快，占用更多内存。
- **动态库**: 运行时加载到内存，占用更少内存，有传递性（PUBLIC 权限）。
## 9. 多行注释

```cmake
#[[
多行注释可以使用 `[[` 和 `]]` 包裹。
]]
```

## 10. 常用命令说明

### 添加可执行文件

```cmake
add_executable( [sources...])
```

### 添加库

```cmake
add_library( [STATIC|SHARED] [sources...])
```

### 链接库

```cmake
target_link_libraries(  )
```

### 包含目录

```cmake
include_directories()
```

### 设置库路径

```cmake
link_directories()
```

通过上述内容，基本覆盖了 CMake 的常见用法，可根据需要进一步扩展学习。


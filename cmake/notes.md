# CMAKE学习

> learn from [cgold](https://cgold.readthedocs.io/en/latest/)  
> `CMake`命令不区分大小写

---

## 1. cmake构建过程

> cmake构建项目分为3个过程  
> 3.3 CMAKE stage

#### 配置过程(Configure step)

cmake检查需要的编译器(`compiler`), 执行项目的`CMakeFile.txt`树, 生成`CMakeCache.txt`.

> 若使用GUI界面可在此阶段修改配置参数等, 命令行则将`Configure`和`Generate`合为一步

---

#### 生成过程(Generate step)

生成编译工具所需要的文件，如: `Visual Studio`的`*.sln`, `Xcode`的`*.xcodeproj`, `Linux`的`Makefile`等


---

#### 编译过程(Build step)

编译生成可执行文件

---

## 2. cmake version和polices

> 3.4 Version and polices

#### cmake_minimum_required

**`cmake_minimun_required`必须是在`CMakeList.txt`中执行的第一个命令**(即：位于根目录的`CMakeList.txt`的第一个命令，*注释不算*)。作用是检查用户使用的`cmake`版本。

---

#### cmake polices

`CMake`决定使用新特性的规则为**`polices`**, 可以在[`cmake-polices`](https://cmake.org/cmake/help/latest/manual/cmake-policies.7.html)中查看各个版本引入的特性。

如果想在一个**新版本中使用老的特性**, 使用[`cmake_policy`](https://cmake.org/cmake/help/latest/command/cmake_policy.html)命令, 例: `cmake_policy(set CMP0038 OLD)`。

如果支持的老的版本(意味着使用老的特性), 但当前`CMake`为较新的版本，则会对老的特性报错, 这时需要根据`CMake`的版本进行设置

```cmake
cmake_minimum_required(VERSION 2.8)
project(foo)

if(POLICY CMP0038)
  # Policy CMP0038 introduced since CMake 3.0 so if we want to be compatible
  # with 2.8 (see cmake_minimum_required) we should put 'cmake_policy' under
  # condition.
  cmake_policy(SET CMP0038 OLD)
endif()

add_library(foo foo.cpp)
target_link_libraries(foo foo) # BAD CODE! Make no sense
```

---

## 3. Project declaration

> 对于一个项目, 必不可少的命令除了`cmake_minimum_required`之外, 就是[`project`](https://cmake.org/cmake/help/latest/command/project.html)命令  
> 3.5 Project declaration

#### project

1. `CMake`使用`project`命令检查项目所需要的运行环境、编译器等。

    > 在`linux`中, `CMake`默认使用的编译器为环境变量中的`CC`和`CXX`

2. 在进行**交叉编译**时, `CMake`在运行`project`命令时检查`tool chain file`。  
指定`tool chain file`在命令行指定`-DCMAKE_TOOLCHAIN_FILE=toolchain.cmake`。

    > 在`project`执行期间, `CMake`会多次读取`tool chain`文件。
    > [`CMAKE_TOOLCHAIN_FILE`](https://cmake.org/cmake/help/latest/variable/CMAKE_TOOLCHAIN_FILE.html?highlight=cmake_toolchain_file)

3. 在`project`中, 可以同时指定项目所使用的语言, 例如:

```cmake
# 只使用C语言
project(foo C)
# 不需要检查编译器(学习、测试时可以使用)
project(foo NONE)
# 使用CMake 3.0+时，添加LANGUAGE关键字
# 但如果只是指定语言则不必添加
project(foo LANGUAGE NONE)
```

4. `project`执行过程中, `CMake`会自动生成一些缓存变量(`Cache Variables`), 例如:

```cmake
cmake_minimum_required(VERSION 3.0)
# 3.0之后, 可以使用VERSION指定版本
project(Foo VERSION 1.2.7)

message("After project:")
# 整个cmake项目的变量, 一共1份, 为顶层project相关信息
message("  Source: ${PROJECT_SOURCE_DIR}")
message("  Binary: ${PROJECT_BINARY_DIR}")
message("  Version: ${PROJECT_VERSION}")
message("  Version (alt): ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}")
# 每一个project都生成的变量(version 为指定才生成)
# foo_{SOURCE,BINARY}_DIR, foo_VERSION_{MAJOR,MINOR,PATCH,TWEAK}
message("  Source: ${foo_SOURCE_DIR")
```

---

## 4. Variables



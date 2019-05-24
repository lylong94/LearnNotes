# CMAKE学习

> learn from [cgold](https://cgold.readthedocs.io/en/latest/)

---

## cmake构建过程

> cmake构建项目分为3个过程  
> 3.1 CMAKE stage

1. 配置过程(Configure step)

    cmake检查需要的编译器(`compiler`), 执行项目的`CMakeFile.txt`树, 生成`CMakeCache.txt`.

    > 若使用GUI界面可在此阶段修改配置参数等, 命令行则将`Configure`和`Generate`合为一步

2. 生成过程(Generate step)

    生成编译工具所需要的文件，如: `Visual Studio`的`*.sln`, `Xcode`的`*.xcodeproj`, `Linux`的`Makefile`等

3. 编译过程(Build step)
    编译生成可执行文件
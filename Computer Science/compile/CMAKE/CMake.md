# CMake
 这是一个文件的依赖关系，也就是说，target这一个或多个的目标文件依赖于prerequisites中的文件，其生成规则定义在command中。说白一点就是说，prerequisites中如果有一个以上的文件比target文件要新的话，command所定义的命令就会被执行。这就是Makefile的规则。

> # CMake 最低版本号要求cmake_minimum_required (VERSION 2.8)# 项目信息project (Demo2)# 查找当前目录下的所有源文件# 并将名称保存到 DIR_SRCS 变量aux_source_directory(. DIR_SRCS)# 指定生成目标add_executable(Demo ${DIR_SRCS})  
#Computer Science/compile/CMAKE#


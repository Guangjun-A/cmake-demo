cmake_minimum_required(VERSION 2.8)

project(DEMO4)
#　把目录设置为当前目录
#　自动添加 CMAKE_CURRENT_BINARY_DIR 和 CMAKE_CURRENT_SOURCE_DIR 到当前处理
#　的 CMakeLists.txt。相当于在每个 CMakeLists.txt 加入:
# INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_CURRENT_SOURCE_DIR})
# Automatically add the current source and build directories to the include path.
set(CMAKE_INCLUDE_CURRENT_DIR ON) 
 # 赋值
option(USE_MY_MATH "USE provided math implementation" ON)

# 把文件config.h.in中的变量替换生成config
configure_file("${PROJECT_SOURCE_DIR}/config.h.in" "${PROJECT_SOURCE_DIR}/config.h") 

if (USE_MYMATH) 
   include_directories("${PROJECT_SOURCE_DIR}/math") # 这样引入的时候就可以写#include<math.h> 而不是#include<Math/math.h>
  add_subdirectory(math) # math在可生成文件所在的文件夹下, add the MathFunctions library
  set (EXTRA_LIBS MathFunctions)
endif (USE_MYMATH)

aux_source_directory(${PROJECT_SOURCE_DIR} DIR_SRCS) # PROJECT_SOURCE_DIR 根目录
MESSAGE(STATUS "==========${PROJECT_SOURCE_DIR}")
MESSAGE(STATUS "==========${PROJECT_BINARY_DIR}")
MESSAGE(STATUS "==========${USE_MY_MATH}")
MESSAGE(STATUS "==========${EXTRA_LIBS}")


add_executable(Demo ${DIR_SRCS})
target_link_libraries(Demo ${EXTRA_LIBS})
# # CMake 最低版本号要求
# cmake_minimum_required (VERSION 2.8)

# # 项目信息
# project (Demo4)

# 设置系统变量CMAKE_INCLUDE_CURRENT_DIR为on就是自动添加
# set(CMAKE_INCLUDE_CURRENT_DIR ON)

# # 是否使用自己的 MathFunctions 库
# option (USE_MYMATH "Use provided math implementation" ON) # 赋值语句，开关作用(f赋值给USE_MYMATH)

# # 加入一个配置头文件，用于处理 CMake 对源码的设置
# # configure_file 命令用于加入一个配置头文件 config.h ，这个文件由 CMake 从 config.h.in 生成，通过这样的机制，将可以通过预定义一些参数和变量来控制代码的生成。
# configure_file (
#   "${PROJECT_SOURCE_DIR}/config.h.in" #　PROJECT_SOURCE_DIR指工程顶层目录
#   "${PROJECT_BINARY_DIR}/config.h"
#   )

# # 是否加入 MathFunctions 库
# if (USE_MYMATH)
#   include_directories ("${PROJECT_SOURCE_DIR}/math") # 创建临时目录索引，这样在main里就可以写#include<MathFunctions.h>而不是#include<math/MathFunctions.h>
#   add_subdirectory (math)
#   set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
# endif (USE_MYMATH)

# # 查找当前目录下的所有源文件
# # 并将名称保存到 DIR_SRCS 变量
# aux_source_directory(. DIR_SRCS)

# # 指定生成目标
# add_executable (Demo ${DIR_SRCS})
# target_link_libraries (Demo  ${EXTRA_LIBS})

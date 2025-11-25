cmake_minimum_required(VERSION 3.16)

project(Aeonovate VERSION 0.1.0 LANGUAGES C CXX)

set(CMAKE_BUILD_PARALLEL_LEVEL 24)

# 指定 C++ 版本
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 20) # C++20：启用Concepts.

# Project 依赖的一些目录
set(PROJECT_EXTERNAL_DIR ${PROJECT_SOURCE_DIR}/external)
set(SRC_DIR ${PROJECT_SOURCE_DIR}/src)
set(PROJECT_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)

# 设置输出路径
set(PROJECT_OUTPUT_DIR ${PROJECT_SOURCE_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_OUTPUT_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_OUTPUT_DIR}/debug)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_OUTPUT_DIR}/release)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_OUTPUT_DIR})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${PROJECT_OUTPUT_DIR}/debug)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RLEASE ${PROJECT_OUTPUT_DIR}/release)
#CMake Debug用
#set(CMAKE_VERBOSE_MAKEFILE ON)

#set(CMAKE_BUILD_TYPE Release)

# project dependencies

## bgfx
set(BGFX_CMAKE_DIR ${PROJECT_EXTERNAL_DIR}/bgfx)
add_subdirectory(${BGFX_CMAKE_DIR})
set(BGFX_DIR ${BGFX_CMAKE_DIR}/bgfx)
set(BIMG_DIR ${BGFX_CMAKE_DIR}/bimg)
set(BX_DIR ${BGFX_CMAKE_DIR}/bx)
set(PROJECT_INCLUDE_DIR ${PROJECT_INCLUDE_DIR} ${BGFX_DIR}/include ${BIMG_DIR}/include ${BX_DIR}/include)

## SDL3
set(SDL3_DIR ${PROJECT_EXTERNAL_DIR}/SDL3)
add_subdirectory(${SDL3_DIR})
set(PROJECT_INCLUDE_DIR ${PROJECT_INCLUDE_DIR} ${SDL3_DIR}/include)


set(PROJECT_INCLUDE_DIR ${PROJECT_INCLUDE_DIR} CACHE STRING "project include dirs")
include_directories(PROJECT_INCLUDE_DIR)

# 子项目
## 基础层
add_subdirectory(${SRC_DIR}/base)
## 核心层
add_subdirectory(${SRC_DIR}/core)
## 接口层
#add_subdirectory(${SRC_DIR}/file)
#add_subdirectory(${SRC_DIR}/net)
## 用户层
#add_subdirectory(${SRC_DIR}/gui)
## 异常处理模块
#add_subdirectory(${SRC_DIR}/log)

# 主程序
add_executable(${PROJECT_NAME} WIN32 src/main.cpp)

# 预编译
target_precompile_headers(${PROJECT_NAME} PUBLIC src/Aeonovate.hpp)

# 添加依赖
set(Aeonovate_LIBRARIES ${Aeonovate_LIBRARIES} bgfx bimg bx SDL3-shared base core)
target_link_libraries(${PROJECT_NAME} ${Aeonovate_LIBRARIES})

target_link_options(${PROJECT_NAME} PRIVATE -t)

include(CTest)
enable_testing()

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

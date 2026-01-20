cmake_minimum_required(VERSION 3.10.0)
project(base VERSION 0.1.0 LANGUAGES C CXX)

include_directories(${SRC_DIR}/base ${PROJECT_INCLUDE_DIR})

add_library(base STATIC base.cpp) #静态库
target_precompile_headers(base PRIVATE pch.h)
target_compile_definitions(base PRIVATE __ANOV_BASE_EXPORTS_)
target_include_directories(base PUBLIC ${SRC_DIR}/base)
target_link_libraries(base PUBLIC SDL3-shared)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

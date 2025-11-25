cmake_minimum_required(VERSION 3.10.0)
project(base VERSION 0.1.0 LANGUAGES C CXX)

include_directories(${SRC_DIR}/base ${PROJECT_INCLUDE_DIR})

add_library(base base.cpp)
target_link_libraries(base PUBLIC SDL3-shared)
target_include_directories(base PUBLIC ${SRC_DIR}/base ${PROJECT_INCLUDE_DIR})

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

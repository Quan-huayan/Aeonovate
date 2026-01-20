cmake_minimum_required(VERSION 3.10.0)
project(core VERSION 0.1.0 LANGUAGES C CXX)

add_library(core core.cpp)
target_include_directories(core PUBLIC ${SRC_DIR}/core)
target_link_libraries(core PUBLIC base)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

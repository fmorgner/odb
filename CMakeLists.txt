cmake_minimum_required(VERSION 2.12)
cmake_policy(SET CMP0042 NEW)
project(odb CXX)

option(WITH_ASAN "Build with the address sanitizer enabled (may need custom toolchain)" OFF)
# option(WITH_CUTE "Build CUTE unit tests" OFF)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0 -g")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3")

if(WITH_ASAN)
  set(CMAKE_BUILD_TYPE "Debug")
  set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fno-omit-frame-pointer -fsanitize=address")
  set(CMAKE_EXE_LINKER_FLAGS "-fsanitize=address")
endif(WITH_ASAN)

add_subdirectory(include)
include_directories(include)
add_subdirectory(source)

add_executable(familytree application/familytree/main.cpp)
add_executable(wb application/wb/main.cpp)
add_executable(xb application/xb/main.cpp)

target_link_libraries(familytree odb)
target_link_libraries(wb odb)
target_link_libraries(xb odb)


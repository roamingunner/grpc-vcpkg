cmake_minimum_required(VERSION 3.10)

set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)
set(CMAKE_CROSSCOMPILING ON CACHE BOOL "")
set(CMAKE_CXX_COMPILER "/home/leo/.local/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf-g++")
set(CMAKE_C_COMPILER "/home/leo/.local/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc")

# set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
# set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
# set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
# set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)

set (CMAKE_CXX_STANDARD 11)
set (CMAKE_BUILD_TYPE Release)
set (CMAKE_PREFIX_PATH "/home/leo/work/vcpkg/installed/a0/share")

project(HelloWorldMinimal)

find_package(gRPC REQUIRED)
pkg_check_modules(pb protobuf REQUIRED)
# find_package(protobuf REQUIRED)
find_package(Threads REQUIRED)

add_subdirectory(src)

# # CMakeLists.txt
# cmake_minimum_required(VERSION 3.10)
# set (CMAKE_CXX_STANDARD 11)
# set (CMAKE_BUILD_TYPE Release)
# set (VCPKG_HOME "/home/leo/work/vcpkg") #IMPORTANT: change this to where your vcpkg exists.
# set (CMAKE_TOOLCHAIN_FILE "${VCPKG_HOME}/scripts/buildsystems/vcpkg.cmake")

# project(HelloWorldMinimal)

# find_package(gRPC CONFIG REQUIRED)
# find_package(Protobuf CONFIG REQUIRED)

# find_package(Threads REQUIRED)

# add_subdirectory(src)

# CMakeLists.txt

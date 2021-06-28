# Copyright 2018 gRPC authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# cmake build file for C++ helloworld example.
# Assumes protobuf and gRPC have been installed using cmake.
# See cmake_externalproject/CMakeLists.txt for all-in-one cmake build
# that automatically builds all the dependencies before building helloworld.

cmake_minimum_required(VERSION 3.5.1)

# Proto file
get_filename_component(hw_proto "helloworld.proto" ABSOLUTE)
get_filename_component(hw_proto_path "${hw_proto}" PATH)
set(VCPKG_HOME "/home/leo/work/vcpkg")
set(PROTOC "${VCPKG_HOME}/installed/x64-linux/tools/protobuf/protoc")
if (NOT EXISTS ${PROTOC})
  message(FATAL_ERROR "The ${PROTOC} program was not found")
endif()
message(STATUS "Found protoc program: " ${PROTOC})

# protoc grpc plugin
set(GRPC_PLUGIN "${VCPKG_HOME}/installed/x64-linux/tools/grpc/grpc_cpp_plugin")
if (NOT EXISTS ${GRPC_PLUGIN})
  message(FATAL_ERROR "The grpc_cpp_plugin plugin was not found, \
                   the gRPC classes are not being generated")
else(EXISTS ${GRPC_PLUGIN})
  message(STATUS "Found grpc_cpp_plugin : " ${GRPC_PLUGIN})
endif()

set(_PROTOBUF_LIBPROTOBUF protobuf::libprotobuf)
# set(_REFLECTION grpc++_reflection)
set(_GRPC_GRPCPP gRPC::grpc++)

# Generated sources
set(hw_proto_srcs "${CMAKE_CURRENT_BINARY_DIR}/helloworld.pb.cc")
set(hw_proto_hdrs "${CMAKE_CURRENT_BINARY_DIR}/helloworld.pb.h")
set(hw_grpc_srcs "${CMAKE_CURRENT_BINARY_DIR}/helloworld.grpc.pb.cc")
set(hw_grpc_hdrs "${CMAKE_CURRENT_BINARY_DIR}/helloworld.grpc.pb.h")
add_custom_command(
      OUTPUT "${hw_proto_srcs}" "${hw_proto_hdrs}" "${hw_grpc_srcs}" "${hw_grpc_hdrs}"
      COMMAND ${PROTOC}
      ARGS --grpc_out "${CMAKE_CURRENT_BINARY_DIR}"
        --cpp_out "${CMAKE_CURRENT_BINARY_DIR}"
        -I "${hw_proto_path}"
        --plugin=protoc-gen-grpc="${GRPC_PLUGIN}"
        "${hw_proto}"
      DEPENDS "${hw_proto}")

# Include generated *.pb.h files
include_directories("${CMAKE_CURRENT_BINARY_DIR}")
include_directories("/home/leo/work/vcpkg/installed/a0/include")
link_directories("/home/leo/work/vcpkg/installed/a0/lib")
# Targets greeter_[async_](client|server)
foreach(_target
  greeter_client greeter_server
  greeter_async_client greeter_async_client2 greeter_async_server)
  add_executable(${_target} "${_target}.cc"
    ${hw_proto_srcs}
    ${hw_grpc_srcs})
  target_link_libraries(${_target}
    ${_GRPC_GRPCPP}
    ${_PROTOBUF_LIBPROTOBUF})
endforeach()

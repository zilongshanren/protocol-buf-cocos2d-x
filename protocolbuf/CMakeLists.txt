#/****************************************************************************
# Copyright (c) 2013 cocos2d-x.org
# Copyright (c) 2014 martell malone
#
# http://www.cocos2d-x.org
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.

# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.
# ****************************************************************************/

# this CMakeLists is to generate libChipmunk.a
cmake_minimum_required(VERSION 2.8)

if(LINUX32)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
elseif(LINUX64)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m64")
endif(LINUX32)

include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/
  ${CMAKE_CURRENT_SOURCE_DIR}
)

file(GLOB_RECURSE protobuf_source_files
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/extension_set.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/generated_message_util.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/message_lite.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/repeated_field.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/wire_format_lite.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/io/coded_stream.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/io/zero_copy_stream_impl_lite.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/io/zero_copy_stream_impl.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/io/zero_copy_stream.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/stubs/common.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/stubs/once.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/stubs/stringprintf.cc"
  "${CMAKE_CURRENT_SOURCE_DIR}/google/protobuf/stubs/atomicops_internals_x86_gcc.cc"
  )


add_library(protobuf STATIC ${protobuf_source_files})

# we must add this options to compile this library
SET_SOURCE_FILES_PROPERTIES( ${protobuf_source_files} PROPERTIES LANGUAGE CXX )


if(ANDROID)
message(STATUS "android doesn't need ARCHIVE_OUTPUT_DIRECTORY")
else()
set_target_properties(protobuf
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
)

endif()


install(DIRECTORY google/protobuf/ DESTINATION "${CMAKE_CURRENT_SOURCE_DIR}/prebuilt/include"
          FILES_MATCHING PATTERN "*.h")

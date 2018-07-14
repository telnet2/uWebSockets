if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    message(FATAL_ERROR "Prevented in-tree built. Please create a build directory outside of the source code and call cmake from there. Thank you.")
endif()

project(xWebSockets)

cmake_minimum_required(VERSION 3.4)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif(NOT CMAKE_BUILD_TYPE)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

set(LIBUV_DEPS_DIR ${CMAKE_SOURCE_DIR}/deps/libuv)

configure_file(cmake/in/libuv.CMakeLists.in ${LIBUV_DEPS_DIR}/CMakeLists.txt)

add_custom_command(
    OUTPUT  ${LIBUV_DEPS_DIR}/CMakeCache.txt
    COMMENT "installing libuv ..."
    COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" . WORKING_DIRECTORY ${LIBUV_DEPS_DIR}
    COMMAND ${CMAKE_COMMAND} --build . WORKING_DIRECTORY ${LIBUV_DEPS_DIR}
)

add_custom_target(libuv_install ALL
    DEPENDS ${LIBUV_DEPS_DIR}/CMakeCache.txt
)

include_directories(${LIBUV_DEPS_DIR}/src/include)
find_library(LIBUV_STATIC_LIBRARY NAMES libuv.a libuv PATHS ${LIBUV_DEPS_DIR}/src PATH_SUFFIXES .libs Release NO_DEFAULT_PATH)
find_library(LIBUV_SHARED_LIBRARY NAMES uv libuv PATHS ${LIBUV_DEPS_DIR}/src PATH_SUFFIXES .libs Release NO_DEFAULT_PATH)

set(OPENSSL_ROOT_DIR /usr/local/opt/openssl)

find_package(OpenSSL REQUIRED) 
if (OpenSSL_FOUND)
    include_directories(${OPENSSL_INCLUDE_DIR})
endif(OpenSSL_FOUND)

find_package(ZLib REQUIRED)
if (ZLIB_FOUND)
    include_directories(${ZLIB_INCLUDE_DIRS})
endif(ZLIB_FOUND)

add_subdirectory(src)
add_subdirectory(examples)
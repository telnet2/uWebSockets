cmake_minimum_required(VERSION 3.5)

project(xWebSockets-Example)

set (CMAKE_CXX_STANDARD 11)

include_directories(${CMAKE_SOURCE_DIR}/src)
add_executable(echo echo.cpp)
target_link_libraries(echo
    PRIVATE xwebsockets
    PRIVATE ${ZLIB_LIBRARIES}
    PRIVATE ${LIBUV_STATIC_LIBRARY}
    PRIVATE ${OPENSSL_CRYPTO_LIBRARY}
    PRIVATE ${OPENSSL_SSL_LIBRARY})
target_include_directories(echo PRIVATE ${CMAKE_SOURCE_DIR}/src)
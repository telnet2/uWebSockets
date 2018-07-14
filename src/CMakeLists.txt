cmake_minimum_required(VERSION 3.5)
project(xWebSockets-Lib)
set (CMAKE_CXX_STANDARD 11)

set(OPENSSL_ROOT_DIR /usr/local/opt/openssl)
find_package(OpenSSL REQUIRED) 
if (OpenSSL_FOUND)
    include_directories(${OPENSSL_INCLUDE_DIR})
    link_directories(${OPENSSL_LIBRARIES})
endif(OpenSSL_FOUND)

if (APPLE)
    set(LIBUV_SRC Libuv.h)
elseif(UNIX)
    set(EPOLL_SRC Epoll.cpp Epoll.h)
endif()


add_library(xwebsockets 
    Asio.h
    Backend.h
    Extensions.cpp Extensions.h
    Group.cpp Group.h
    HTTPSocket.cpp HTTPSocket.h
    Hub.cpp Hub.h
    Networking.cpp Networking.h
    Node.cpp Node.h
    Room.cpp Room.h
    Socket.cpp Socket.h
    WebSocket.cpp WebSocket.h
    WebSocketProtocol.h
    uWS.h
    ${LIBUV_SRC}
    ${EPOLL_SRC})

if (APPLE)
add_dependencies(xwebsockets libuv_install)
endif()

set(XWEBSOCKET_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/src)
set(XWEBSOCKET_LIBRARIES xwebsockets)
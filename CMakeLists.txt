cmake_minimum_required(VERSION 3.20)

project(redis_clone)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(server
        server.cpp
)

add_executable(client
        client.cpp
)
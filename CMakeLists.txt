cmake_minimum_required(VERSION 3.10.2)
project(clion-gateway)

set(CMAKE_CXX_STANDARD 14)

include_directories(.)

add_executable(clion-gateway
    main.cpp)
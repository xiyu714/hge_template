cmake_minimum_required(VERSION 2.8)
cmake_policy(SET CMP0015 NEW)
set(Project_name "hge_default")
project(${Project_name})

set(CMAKE_CXX_STANDARD 14)

include_directories(./include)
link_directories(./lib)
link_libraries(hge)
FILE(COPY dlls/hge.dll DESTINATION "${CMAKE_BINARY_DIR}")
add_executable(${Project_name} main.cpp)
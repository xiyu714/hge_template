cmake_minimum_required(VERSION 2.8)

# suppress link_directories() treats paths relative to the source dir warning
cmake_policy(SET CMP0015 NEW)
# set project name
set(Project_name "hge_default")
project(${Project_name})

set(CMAKE_CXX_STANDARD 14)

include_directories(./include)

link_directories(./lib)
link_libraries(hge)

# copy all files in dlls to binary_dir
file(GLOB DLLS "dlls/*.dll")
FILE(COPY ${DLLS} DESTINATION "${CMAKE_BINARY_DIR}")

add_executable(${Project_name} main.cpp)
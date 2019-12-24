cmake_minimum_required(VERSION 2.8)

# suppress link_directories() treats paths relative to the source dir warning
cmake_policy(SET CMP0015 NEW)
cmake_policy(SET CMP0054 NEW)
# set project name
set(Project_name "hge_default")
project(${Project_name})

set(CMAKE_CXX_STANDARD 14)

include_directories(./include)
if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    # using Clang
    link_directories(./lib/gcc)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # using GCC
    link_directories(./lib/gcc)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    # using Visual Studio C++
    link_directories(./lib/vc)
else()
    link_directories(./lib/gcc)
    MESSAGE(SEND_ERROR "use ./lib/gcc; may not support this compiler")
endif()
link_libraries(hge hgehelp)

# copy all files in dlls to binary_dir/bin
file(GLOB DLLS "dlls/*.dll")
FILE(COPY ${DLLS} DESTINATION "${CMAKE_BINARY_DIR}/bin")
# copy res directory to binary_dir/bin
FILE(COPY res DESTINATION "${CMAKE_BINARY_DIR}/bin")
# output binanry file in bin directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_executable(${Project_name} main.cpp)
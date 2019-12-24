cmake_minimum_required(VERSION 2.8)

# suppress link_directories() treats paths relative to the source dir warning
cmake_policy(SET CMP0015 NEW)
cmake_policy(SET CMP0054 NEW)
# set project name
set(Project_name "hge_default")
project(${Project_name})

set(CMAKE_CXX_STANDARD 14)

include_directories(./include)
if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang" OR CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # using GCC OR Clang
    link_directories(./lib/gcc)
    link_libraries(hge)
    MESSAGE(WARNING "There is not libhgehelp.a")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    # using Visual Studio C++
    SET(CMAKE_EXE_LINKER_FLAGS /NODEFAULTLIB:LIBC;)
    link_directories(./lib/vc)
    # VS2015 need legacy_stdio_definitions ; ref:https://stackoverflow.com/questions/32418766/c-unresolved-external-symbol-sprintf-and-sscanf-in-visual-studio-2015
    link_libraries(hge hgehelp legacy_stdio_definitions)
else()
    link_directories(./lib/gcc)
    link_libraries(hge)
    MESSAGE(WARNING "There is not libhgehelp.a")
    MESSAGE(WARNING "use ./lib/gcc; may not support this compiler")
endif()

# copy all files in dlls to binary_dir/bin
file(GLOB DLLS "dlls/*.dll")
FILE(COPY ${DLLS} DESTINATION "${CMAKE_BINARY_DIR}/bin")
# copy res directory to binary_dir/bin
FILE(COPY res DESTINATION "${CMAKE_BINARY_DIR}/bin")
# output binanry file in bin directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_executable(${Project_name} main.cpp)
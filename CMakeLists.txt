cmake_minimum_required(VERSION 3.12)
project(os-mycat CXX)

set(CMAKE_CXX_STANDARD 17)

# Options
option(WARNINGS_AS_ERRORS   "Treat compiler warnings as errors."    OFF)
option(ENABLE_SANITIZERS    "Use sanitizers to detect errors."      OFF)
option(ENABLE_CONAN         "Use Conan as a package manager."       OFF)

set(Boost_USE_STATIC_LIBS ON)
set(Boost_USE_MULTITHREADED ON)
#set(Boost_USE_STATIC_RUNTIME ON)
#set(BOOST_ROOT C:/Program Files/boost/boost_1_71_0)
#set(BOOST_INCLUDEDIR C:/Program Files/boost/boost_1_71_0)
#set(BOOST_LIBRARYDIR C:/Program Files/boost/boost_1_71_0/stage/lib)
# Include fixed CMake configuration
# include(cmake/config.cmake)

# Project source compilation
include_directories(inc)
add_library(file_io STATIC src/file_io.cpp inc/operations/file_io.hpp)

find_package(Boost 1.71.0 COMPONENTS program_options REQUIRED)
include_directories(${Boost_INCLUDE_DIR})

add_executable(mycat src/main.cpp)

target_link_libraries(mycat file_io)
target_link_libraries(mycat Boost::program_options)


# Include default CMake configuration
include(cmake/config.cmake)

# Sanitizers configuration
if (ENABLE_SANITIZERS)
    set(SANITIZE_UNDEFINED ON)
    # Only one of Memory, Address, or Thread sanitizers is applicable at the time
    set(SANITIZE_MEMORY ON)
    #    set(SANITIZE_ADDRESS ON)
    #    set(SANITIZE_THREAD ON)
    find_package(Sanitizers)

    # CMake targets to which apply sanitizers
    add_sanitizers(file_io add)
endif ()
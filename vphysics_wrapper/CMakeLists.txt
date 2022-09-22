# CMake script for the vphysics_jolt_wrapper module

cmake_minimum_required(VERSION 3.16)
project(vphysics_jolt_wrapper)

set(SRCDIR "${CMAKE_CURRENT_SOURCE_DIR}/../..")
set(CMAKE_MODULE_PATH "${SRCDIR}/cmake")
set(OUTBINNAME "vphysics")

include(source_dll_base)


## Source Files ##

target_sources(${PROJECT_NAME} PRIVATE "vphysics_wrapper.cpp")


## Header Files ##

file(GLOB SRC_HEADER_FILES "*.h")
target_sources(${PROJECT_NAME} PRIVATE ${SRC_HEADER_FILES})


## Link Libraries ##

# System libs must be linked last!
target_link_libraries(${PROJECT_NAME} ${SYSTEM_LIBS})

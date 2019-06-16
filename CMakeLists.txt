cmake_minimum_required(VERSION 3.14.5)
project(minispaserver CXX)

include(ExternalProject)
find_package(Git REQUIRED)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# ------------------------------------------------------------------------------
# Includes
# ------------------------------------------------------------------------------

include_directories(include)
include_directories(SYSTEM ${CMAKE_BINARY_DIR}/include)

# ------------------------------------------------------------------------------
# Boost needed by the C++ REST SDK
# ------------------------------------------------------------------------------

find_package(Boost 1.70.0 COMPONENTS thread REQUIRED)

# ------------------------------------------------------------------------------
# C++ REST SDK
# ------------------------------------------------------------------------------

find_package(cpprestsdk REQUIRED cpprest)

#[[
set(PISTACHE_CMAKE_ARGS -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=${CMAKE_BINARY_DIR})

ExternalProject_Add(
        pistache
        GIT_REPOSITORY      https://github.com/oktal/pistache.git
        GIT_TAG             master
        GIT_SHALLOW         1
        CMAKE_ARGS          ${PISTACHE_CMAKE_ARGS}
        PREFIX              ${CMAKE_BINARY_DIR}/external/pistache/prefix
        TMP_DIR             ${CMAKE_BINARY_DIR}/external/pistache/tmp
        STAMP_DIR           ${CMAKE_BINARY_DIR}/external/pistache/stamp
        DOWNLOAD_DIR        ${CMAKE_BINARY_DIR}/external/pistache/download
        SOURCE_DIR          ${CMAKE_BINARY_DIR}/external/pistache/src
        BINARY_DIR          ${CMAKE_BINARY_DIR}/external/pistache/build
)
]]

# ------------------------------------------------------------------------------
# Catch
# ------------------------------------------------------------------------------

list(APPEND CATCH_CMAKE_ARGS
        "-DCMAKE_INSTALL_PREFIX=${CMAKE_BINARY_DIR}"
        )

ExternalProject_Add(
        catch
        GIT_REPOSITORY      https://github.com/Bareflank/catch.git
        GIT_TAG             v1.2
        GIT_SHALLOW         1
        CMAKE_ARGS          ${CATCH_CMAKE_ARGS}
        PREFIX              ${CMAKE_BINARY_DIR}/external/catch/prefix
        TMP_DIR             ${CMAKE_BINARY_DIR}/external/catch/tmp
        STAMP_DIR           ${CMAKE_BINARY_DIR}/external/catch/stamp
        DOWNLOAD_DIR        ${CMAKE_BINARY_DIR}/external/catch/download
        SOURCE_DIR          ${CMAKE_BINARY_DIR}/external/catch/src
        BINARY_DIR          ${CMAKE_BINARY_DIR}/external/catch/build
)

# ------------------------------------------------------------------------------
# Main Executable
# ------------------------------------------------------------------------------


set(SOURCE_FILES src/main.cpp)
add_executable(minispaserver ${SOURCE_FILES})
target_link_libraries(minispaserver PRIVATE cpprestsdk::cpprest ${Boost_LIBRARIES})

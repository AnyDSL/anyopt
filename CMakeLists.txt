cmake_minimum_required(VERSION 3.18.4 FATAL_ERROR)
project(anyopt)

set(PROJECT_VERSION_MAJOR 0)
set(PROJECT_VERSION_MINOR 1)
#set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "limited config" FORCE)

option(BUILD_SHARED_LIBS "Build shared libraries" ON)
#option(CODE_COVERAGE "Enable code coverage using gcov in Debug builds" OFF)
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS 1)

if (CMAKE_BUILD_TYPE STREQUAL "")
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Debug or Release" FORCE)
endif()

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Compile for code coverage in Debug mode
#list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)
#if (CODE_COVERAGE AND CMAKE_BUILD_TYPE STREQUAL "Debug")
#    include(CodeCoverage)
#    append_coverage_compiler_flags()
#endif()

set(COLOR_TTY_AVAILABLE TRUE)
#if (WIN32)
#    # By default, Windows console does not support ANSI escape codes
#    set(COLOR_TTY_AVAILABLE FALSE)
#endif ()
set(COLORIZE ${COLOR_TTY_AVAILABLE} CACHE BOOL "Set to TRUE to enable colorized output. Requires an ANSI compliant terminal.")

find_package(Thorin REQUIRED)
if(NOT TARGET nlohmann_json)
    find_package(nlohmann_json 3.2.0 REQUIRED)
endif()

add_subdirectory(src)
#if (BUILD_TESTING)
#    include(CTest)
#    add_subdirectory(test)
#endif ()

export(TARGETS libanyopt anyopt FILE ${CMAKE_BINARY_DIR}/share/anydsl/cmake/anyopt-exports.cmake)
configure_file(cmake/anyopt-config.cmake.in ${CMAKE_BINARY_DIR}/share/anydsl/cmake/anyopt-config.cmake @ONLY)

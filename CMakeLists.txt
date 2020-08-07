cmake_minimum_required(VERSION 3.15)

project(cpp-project LANGUAGES CXX)

set(CMAKE_VERBOSE_MAKEFILE OFF)

site_name(HOSTNAME)

option(ENABLE_TESTING "Enable Test Builds" OFF)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)

include(StandardProjectSettings)

# Link this 'library' to set the c++ standard / compile-time options requested
add_library(project_options INTERFACE)
target_compile_features(project_options INTERFACE cxx_std_17)

# Link this 'library' to use the warnings specified in CompilerWarnings.cmake
add_library(project_warnings INTERFACE)

# standard compiler warnings
include(CompilerWarnings)
set_project_warnings(project_warnings)

# sanitizer options if supported by compiler
include(Sanitizers)
enable_sanitizers(project_options)

# enable doxygen
include(Doxygen)
enable_doxygen()

# allow for static analysis options
include(StaticAnalyzers)
include(ProhibitInSourceBuild)
include(SetupNinjaColors)
include(SetupConan)

add_subdirectory(src)

if(ENABLE_TESTING)
  enable_testing()
  message(
    "Building Unit Tests."
  )
  add_subdirectory(test)
endif()

# rainbomizer
cmake_minimum_required(VERSION 3.1.5)
project(rainbomizer)

# C++20
if (MSVC_VERSION GREATER_EQUAL "1900")
  include(CheckCXXCompilerFlag)
  CHECK_CXX_COMPILER_FLAG("/std:c++latest" _cpp_latest_flag_supported)
  if (_cpp_latest_flag_supported)
    add_compile_options("/std:c++latest")
  endif()
  set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
  # Because CMAKE_MSVC_RUNTIME_LIBRARY doesn't work for whatever reason
  set(CompilerFlags
    CMAKE_CXX_FLAGS
    CMAKE_CXX_FLAGS_DEBUG
    CMAKE_CXX_FLAGS_RELEASE
    CMAKE_C_FLAGS
    CMAKE_C_FLAGS_DEBUG
    CMAKE_C_FLAGS_RELEASE
    )
  foreach(CompilerFlag ${CompilerFlags})
    string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
  endforeach()
endif()

set (CMAKE_CXX_STANDARD 14)

# Sources
include_directories(include)
file(GLOB SOURCES src/*.cc src/util/*.cc)

add_library("rainbomizer" SHARED ${SOURCES})

# Properties
set_target_properties("rainbomizer" PROPERTIES SUFFIX ".asi")
target_link_libraries("rainbomizer" PUBLIC dbghelp wininet)
target_compile_definitions("rainbomizer" PUBLIC NOMINMAX)

if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  
else()
  target_compile_features("rainbomizer" PRIVATE cxx_auto_type)
endif()

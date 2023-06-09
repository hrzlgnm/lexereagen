cmake_minimum_required(VERSION 3.18)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
include(cmake/prelude.cmake)

project(
    lexereagen
    VERSION 0.1.0
    DESCRIPTION "The name is lexereagen"
    HOMEPAGE_URL "https://github.com/hrzlgnm/lexereagen"
    LANGUAGES CXX
)

include(cmake/project-is-top-level.cmake)
include(cmake/variables.cmake)

# ---- Declare library ----

add_library(lexereagen_lib 
    OBJECT
        source/ast.cpp
        source/lexer.cpp
        source/object.cpp
        source/parser.cpp
        source/token.cpp
        source/token_type.cpp
)

target_precompile_headers(lexereagen_lib
    PRIVATE
        <algorithm>
        <iostream>
        <cstdint>
        <memory>
        <string>
        <string_view>
)

target_include_directories(lexereagen_lib ${warning_guard}
    PUBLIC
        "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/source>"
)
target_compile_features(lexereagen_lib PUBLIC cxx_std_20)

# ---- Declare executable ----

add_executable(lexereagen_exe source/main.cpp)
add_executable(lexereagen::exe ALIAS lexereagen_exe)

set_property(TARGET lexereagen_exe PROPERTY OUTPUT_NAME lexereagen)

target_compile_features(lexereagen_exe PRIVATE cxx_std_20)

target_link_libraries(lexereagen_exe PRIVATE lexereagen_lib)

# ---- Install rules ----

if(NOT CMAKE_SKIP_INSTALL_RULES)
  include(cmake/install-rules.cmake)
endif()

# ---- Developer mode ----

if(NOT lexereagen_DEVELOPER_MODE)
  return()
elseif(NOT PROJECT_IS_TOP_LEVEL)
  message(
      AUTHOR_WARNING
      "Developer mode is intended for developers of lexereagen"
  )
endif()

include(cmake/dev-mode.cmake)

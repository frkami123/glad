cmake_minimum_required(VERSION 3.24)
set(CMAKE_MODULE_PATH ${EAO_CMAKE_MODULE_PATH} ${CMAKE_MODULE_PATH})

set(PROJECT_NAME "glad")

project(${PROJECT_NAME} VERSION 0.0.1
  DESCRIPTION "${PROJECT_NAME}")

set(CMAKE_POLICY_DEFAULT_CMP0135 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0048 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0077 NEW)

option(GLAD_PLATFORM_EMSCRIPTEN "Glad on emscripten platform" OFF)
option(GLAD_GLX_NO_X11 "No idea what this is" OFF)

add_library(glad STATIC
  include/glad/gl.h
  include/glad/glx.h
  include/KHR/khrplatform.h
)

target_sources(glad PRIVATE
  src/gl.c
  src/glx.c
)

set_property(TARGET glad PROPERTY CXX_STANDARD 23)
target_compile_features(glad INTERFACE cxx_std_23)
if(NOT WIN32)
  target_compile_options(glad PRIVATE -fPIC)
endif()
target_compile_definitions(glad PRIVATE $<IF:$<BOOL:GLAD_PLATFORM_EMSCRIPTEN>,"GLAD_PLATFORM_EMSCRIPTEN","">)

target_include_directories(
  glad
  PUBLIC
  $<BUILD_INTERFACE:${${PROJECT_NAME}_SOURCE_DIR}/include>
)

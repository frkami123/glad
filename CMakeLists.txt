cmake_minimum_required(VERSION 3.24)
set(PROJECT_NAME "glad")

project(glad VERSION 0.0.1 DESCRIPTION "glad")

set(CMAKE_POLICY_DEFAULT_CMP0135 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0048 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0077 NEW)

option(GLAD_PLATFORM_EMSCRIPTEN "Glad on emscripten platform" OFF)

add_library(glad STATIC)

target_sources(glad
  PUBLIC
    FILE_SET HEADERS 
    BASE_DIRS include
    FILES 
    include/glad/gl.h
    $<$<PLATFORM_ID:Linux>:include/glad/glx.h>
    $<$<PLATFORM_ID:Windows>:include/glad/wgl.h>
    include/KHR/khrplatform.h
  PRIVATE
    src/gl.c
    $<$<PLATFORM_ID:Linux>:$<IF:$<BOOL:GLAD_PLATFROM_GLX>,src/glx.c,"">>
    $<$<PLATFORM_ID:Windows>:$<IF:$<BOOL:GLAD_PLATFROM_WGL>,src/wgl.c,"">>
)

set_property(TARGET glad PROPERTY C_STANDARD 17)
target_compile_features(glad INTERFACE c_std_17)

target_compile_definitions(glad PRIVATE $<IF:$<BOOL:GLAD_PLATFORM_EMSCRIPTEN>,"GLAD_PLATFORM_EMSCRIPTEN","">)

target_include_directories(glad
  PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
)

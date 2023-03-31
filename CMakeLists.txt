cmake_minimum_required(VERSION 3.20)

set(PROJECT_NAME "ReaperExampleExtension")

project(${PROJECT_NAME}
  VERSION 0.0.1
  DESCRIPTION "REAPER extension template using ReaImGui with CMake"
  LANGUAGES CXX
)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

if (APPLE)
    set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64" CACHE STRING "macOS architecture" FORCE)
    set(CMAKE_OSX_DEPLOYMENT_TARGET "10.11" CACHE STRING "Minimum OS X deployment version" FORCE)
endif ()

if (WIN32)
    add_definitions(-DUNICODE -D_UNICODE)
endif ()

add_library(${PROJECT_NAME} SHARED ./hello_world.cpp)
target_include_directories(${PROJECT_NAME} PRIVATE
        ./WDL/WDL
        ./reaper-sdk/sdk)

if (APPLE)
    set_target_properties(${PROJECT_NAME} PROPERTIES SUFFIX ".dylib")
endif ()

if (WIN32)
    if ("${CMAKE_GENERATOR_PLATFORM}" MATCHES "Win32")
        set_target_properties(${PROJECT_NAME} PROPERTIES SUFFIX "_w32.dll")
    else ()
        set_target_properties(${PROJECT_NAME} PROPERTIES SUFFIX "_x64.dll")
    endif ()
endif ()
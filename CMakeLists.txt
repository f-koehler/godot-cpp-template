cmake_minimum_required(VERSION 3.0)
project(GodotCppTemplate)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include_directories(${CMAKE_SOURCE_DIR}/external/godot-cpp/include)
include_directories(${CMAKE_SOURCE_DIR}/external/godot-cpp/include/core)
include_directories(${CMAKE_SOURCE_DIR}/external/godot-cpp/include/gen)
include_directories(${CMAKE_SOURCE_DIR}/external/godot-cpp/godot_headers)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
link_directories(${CMAKE_SOURCE_DIR}/external/godot-cpp/bin)

include_directories(${CMAKE_SOURCE_DIR}/include)

include(CMakeParseArguments)
function(add_godot_library)
    cmake_parse_arguments(
        PARSED_ARGS
        ""
        "NAME"
        "SRCS"
        ${ARGN}
    )
    if(NOT PARSED_ARGS_NAME)
        message(FATAL_ERROR "No library name specified")
    endif()
    if(NOT PARSED_ARGS_SRCS)
        message(FATAL_ERROR "No source files specified")
    endif()
    add_library(${PARSED_ARGS_NAME} SHARED ${PARSED_ARGS_SRCS})
    target_link_libraries(${PARSED_ARGS_NAME} godot-cpp.linux.debug.64)
endfunction()

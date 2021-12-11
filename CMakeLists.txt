cmake_minimum_required(VERSION 3.21 FATAL_ERROR)

set(CMAKE_BUILD_TYPE Release)

project("LNK4206" LANGUAGES CXX)

if(OPTION_STATIC)
	set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded" CACHE STRING "")
else()
	set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreadedDLL" CACHE STRING "")
endif()

if(OPTION_LTCG)
	set(CMAKE_INTERPROCEDURAL_OPTIMIZATION ON CACHE BOOL "")
endif()

if(OPTION_WITH_PCH)
	set(CMAKE_DISABLE_PRECOMPILE_HEADERS OFF)
else()
	set(CMAKE_DISABLE_PRECOMPILE_HEADERS ON)
endif()

if(OPTION_Z7)
	add_compile_options("/Zi")
else()
	add_compile_options("/Z7")
endif()

if(OPTION_WX)
	add_link_options("/WX")
endif()

if(OPTION_FASTLINK)
	add_link_options("/DEBUG:FASTLINK")
else()
	add_link_options("/DEBUG:FULL")
endif()

add_subdirectory(lib)
add_subdirectory(src)

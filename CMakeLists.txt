cmake_minimum_required(VERSION 3.21 FATAL_ERROR)

set(CMAKE_BUILD_TYPE Release)

project("LNK4206" LANGUAGES CXX)

set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded" CACHE STRING "")

#add_compile_options("/Gw;/O2;/Ob3")

if(NOT DEFINED CMAKE_INTERPROCEDURAL_OPTIMIZATION AND NOT DEFINED CMAKE_INTERPROCEDURAL_OPTIMIZATION_RELEASE)
	set(CMAKE_INTERPROCEDURAL_OPTIMIZATION_RELEASE ON CACHE BOOL "")
endif()
	
add_link_options("/WX")

#add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:$<IF:$<CONFIG:Debug>,/ZI,/Z7>;/FS>")
add_compile_options("$<IF:$<IN_LIST:$<TARGET_PROPERTY:TYPE>,SHARED_LIBRARY;EXECUTABLE;MODULE_LIBRARY>,/Zi,/Z7>;/FS")

add_link_options("/DEBUG:FASTLINK")
#add_link_options("/DEBUG:$<IF:$<IN_LIST:lib::lib,$<TARGET_PROPERTY:LINK_LIBRARIES>>,FULL,FASTLINK>")

add_subdirectory(lib)
add_subdirectory(src)

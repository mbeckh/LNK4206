cmake_minimum_required(VERSION 3.21 FATAL_ERROR)

set(CMAKE_BUILD_TYPE Release)

project("LNK4206" LANGUAGES CXX)

set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>" CACHE STRING "")

#add_compile_options("$<$<COMPILE_LANGUAGE:CXX>:/EHsc;/GR-;/permissive-;$<$<CONFIG:Release>:/Zc:inline>>")
add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:$<$<CONFIG:Debug>:/Od>;$<$<CONFIG:Release>:/Gw;/O2;/Ob3>>")
add_link_options("$<$<CONFIG:Release>:/OPT:ICF;/OPT:REF>")

if(NOT DEFINED CMAKE_INTERPROCEDURAL_OPTIMIZATION AND NOT DEFINED CMAKE_INTERPROCEDURAL_OPTIMIZATION_RELEASE)
	set(CMAKE_INTERPROCEDURAL_OPTIMIZATION_RELEASE ON CACHE BOOL "")
endif()
	
add_link_options("$<$<CONFIG:Release>:/WX>")

#add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:$<IF:$<CONFIG:Debug>,/ZI,/Z7>;/FS>")
add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:$<IF:$<CONFIG:Debug>,/ZI,$<IF:$<IN_LIST:$<TARGET_PROPERTY:TYPE>,SHARED_LIBRARY;EXECUTABLE;MODULE_LIBRARY>,/Zi,/Z7>>;/FS>")

#add_link_options("/DEBUG:FASTLINK")
add_link_options("/DEBUG:$<IF:$<OR:$<NOT:$<CONFIG:Debug>>,$<IN_LIST:lib::lib,$<TARGET_PROPERTY:LINK_LIBRARIES>>>,FULL,FASTLINK>")

add_subdirectory(lib)
add_subdirectory(src)

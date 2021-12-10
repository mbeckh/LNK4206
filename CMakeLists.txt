cmake_minimum_required(VERSION 3.21 FATAL_ERROR)

set(CMAKE_BUILD_TYPE Release)

project("LNK4206" LANGUAGES CXX)

set(CMAKE_OPTIMIZE_DEPENDENCIES ON CACHE BOOL "")

add_compile_definitions(UNICODE=1 _UNICODE=1
						WIN32=1 _WINDOWS=1
						"$<$<CONFIG:Debug>:_DEBUG=1>" "$<$<CONFIG:Release>:NDEBUG=1>"
						WINVER=0x0A00 _WIN32_WINNT=0x0A00)

set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>" CACHE STRING "")

foreach(suffix "" "_DEBUG" "_RELEASE")
	foreach(language C CXX)
		if(CMAKE_${language}_FLAGS${suffix})
			string(REGEX REPLACE "(^| )[/-]((D(WIN32|_WINDOWS|_?UNICODE|[_N]DEBUG|WINVER|_WIN32_WINNT)(=[^ ]*)?)|bigobj|GR-?|EH([sc]|sc)|M[DT]d?|O[12dgistxy]|Ob[0-9]|RTC1|utf-8|w|W[0-4]|Z[7iI])($| )" " " flags "${CMAKE_${language}_FLAGS${suffix}}")
			string(STRIP "${flags}" flags)
			string(REGEX REPLACE "  +" " " flags "${flags}")
			set(CMAKE_${language}_FLAGS${suffix} "${flags}" CACHE STRING "" FORCE)
		endif()
	endforeach()

	foreach(output EXE SHARED MODULE STATIC)
		if(CMAKE_${output}_LINKER_FLAGS${suffix})
			string(REGEX REPLACE "(^| )[/-](([Dd][Ee][Bb][Uu][Gg]|[Ii][Nn][Cc][Rr][Ee][Mm][Ee][Nn][Tt][Aa][Ll]|[Ll][Tt][Cc][Gg]|[Oo][Pp][Tt])(:[^ ]*)?)($| )" " " flags "${CMAKE_${output}_LINKER_FLAGS${suffix}}")
			string(STRIP "${flags}" flags)
			string(REGEX REPLACE "  +" " " flags "${flags}")
			set(CMAKE_${output}_LINKER_FLAGS${suffix} "${flags}" CACHE STRING "" FORCE)
		endif()
	endforeach()
endforeach()

add_compile_options("$<$<COMPILE_LANGUAGE:CXX>:/EHsc;/GR-;/permissive-;$<$<CONFIG:Release>:/Zc:inline>>")
add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:$<$<CONFIG:Debug>:/Od>;$<$<CONFIG:Release>:/Gw;/O2;/Ob3>>")
add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:$<$<CONFIG:Debug>:/RTC1>;/sdl>")
add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:/W4;/wd4373;$<$<CONFIG:Release>:/WX>>")
add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:/nologo;/bigobj;/diagnostics:caret;/utf-8>")
add_link_options("$<$<CONFIG:Release>:/OPT:ICF;/OPT:REF>")

if(NOT DEFINED CMAKE_INTERPROCEDURAL_OPTIMIZATION AND NOT DEFINED CMAKE_INTERPROCEDURAL_OPTIMIZATION_RELEASE)
	set(CMAKE_INTERPROCEDURAL_OPTIMIZATION_RELEASE ON CACHE BOOL "")
endif()
	
#add_link_options(/PDBALTPATH:%_PDB%)

add_link_options(/NOLOGO "$<$<NOT:$<OR:$<BOOL:$<TARGET_PROPERTY:INTERPROCEDURAL_OPTIMIZATION>>,$<BOOL:$<TARGET_PROPERTY:INTERPROCEDURAL_OPTIMIZATION_$<UPPER_CASE:$<CONFIG>>>>>>:/INCREMENTAL$<$<CONFIG:Release>::NO>>")
if(NOT CMAKE_STATIC_LINKER_FLAGS MATCHES "(^| )[/-][Nn][Oo][Ll][Oo][Gg][Oo]( |$)")
	string(APPEND CMAKE_STATIC_LINKER_FLAGS " /NOLOGO")
	string(STRIP "${CMAKE_STATIC_LINKER_FLAGS}" CMAKE_STATIC_LINKER_FLAGS)
	set(CMAKE_STATIC_LINKER_FLAGS "${CMAKE_STATIC_LINKER_FLAGS}" CACHE STRING "" FORCE)
endif()

add_link_options("$<$<CONFIG:Release>:/WX>")
if(NOT CMAKE_STATIC_LINKER_FLAGS_RELEASE MATCHES "(^| )[/-]WX( |$)")
	string(APPEND CMAKE_STATIC_LINKER_FLAGS_RELEASE " /WX")
	string(STRIP "${CMAKE_STATIC_LINKER_FLAGS_RELEASE}" CMAKE_STATIC_LINKER_FLAGS_RELEASE)
	set(CMAKE_STATIC_LINKER_FLAGS_RELEASE "${CMAKE_STATIC_LINKER_FLAGS_RELEASE}" CACHE STRING "" FORCE)
endif()

if(CMAKE_BUILD_TYPE MATCHES "[Dd][Ee]?[Bb][Uu]?[Gg]" OR CMAKE_BUILD_TYPE MATCHES "[Ww][Ii][Tt][Hh][Dd][Ee][Bb][Ii][Nn][Ff][Oo]")
	set(CMAKE_VS_JUST_MY_CODE_DEBUGGING ON CACHE BOOL "")
endif()

add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:/experimental:external;/external:W0>")
set(CMAKE_INCLUDE_SYSTEM_FLAG_CXX "/external:I " CACHE STRING "" FORCE)


#add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:$<IF:$<CONFIG:Debug>,/ZI,/Z7>;/FS>")
#add_compile_options("$<$<COMPILE_LANGUAGE:C,CXX>:$<IF:$<CONFIG:Debug>,/ZI,/Z7>>")

add_compile_options("$<$<AND:$<COMPILE_LANGUAGE:C,CXX>,$<OR:$<IN_LIST:lib::lib,$<TARGET_PROPERTY:LINK_LIBRARIES>>>>:/FC>"
					"$<$<AND:$<COMPILE_LANGUAGE:C,CXX>,$<NOT:$<OR:$<CONFIG:Debug>,$<IN_LIST:lib::lib,$<TARGET_PROPERTY:LINK_LIBRARIES>>>>>:/d1trimfile:$<SHELL_PATH:$<TARGET_PROPERTY:SOURCE_DIR>/>>"
					"$<$<AND:$<COMPILE_LANGUAGE:C,CXX>,$<NOT:$<OR:$<CONFIG:Debug>,$<IN_LIST:lib::lib,$<TARGET_PROPERTY:LINK_LIBRARIES>>>>>:/d1trimfile:$<SHELL_PATH:$<TARGET_PROPERTY:BINARY_DIR>/>>")

add_link_options("/DEBUG:FASTLINK")
#add_link_options("/DEBUG:$<IF:$<OR:$<NOT:$<CONFIG:Debug>>,$<IN_LIST:lib::lib,$<TARGET_PROPERTY:LINK_LIBRARIES>>>,FULL,FASTLINK>")

add_subdirectory(lib)
add_subdirectory(src)

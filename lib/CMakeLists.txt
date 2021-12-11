add_library(lib "lib.cpp")
add_library(lib::lib ALIAS lib)

target_compile_definitions(lib PRIVATE WIN32_LEAN_AND_MEAN=1 NOMINMAX=1)
target_compile_features(lib PUBLIC cxx_std_20)
target_precompile_headers(lib PRIVATE "pch.h")
#target_compile_options(lib PRIVATE /Ylfoo)

target_include_directories(lib PUBLIC "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>" "include")

set_target_properties(lib PROPERTIES
    DEBUG_POSTFIX d
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

#set_target_properties(lib PROPERTIES
#	COMPILE_PDB_NAME "" # use default
#	COMPILE_PDB_OUTPUT_DIRECTORY "$<TARGET_FILE_DIR:lib>")

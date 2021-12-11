add_library(lib "lib.cpp")
add_library(lib::lib ALIAS lib)

target_compile_features(lib PUBLIC cxx_std_20)
target_precompile_headers(lib PRIVATE "pch.h")

target_include_directories(lib PUBLIC "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>")

set_target_properties(lib PROPERTIES
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

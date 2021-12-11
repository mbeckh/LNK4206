add_executable(LNK4206 "main.cpp")

target_compile_features(LNK4206 PRIVATE cxx_std_20)
target_precompile_headers(LNK4206 PRIVATE "pch.h")

set_target_properties(LNK4206 PROPERTIES
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

target_link_libraries(LNK4206 PRIVATE lib::lib)

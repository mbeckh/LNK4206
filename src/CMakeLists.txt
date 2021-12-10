add_executable(LNK4206 "main.cpp")

target_compile_definitions(LNK4206 PRIVATE WIN32_LEAN_AND_MEAN=1 NOMINMAX=1)
target_compile_features(LNK4206 PRIVATE cxx_std_20)
target_precompile_headers(LNK4206 PRIVATE "pch.h")

set_target_properties(LNK4206 PROPERTIES
    DEBUG_POSTFIX d
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

target_link_libraries(LNK4206 PRIVATE lib::lib)

#target_link_options(LNK4206 PRIVATE "/d2:-SSAOptimizer-")

set_target_properties(LNK4206 PROPERTIES
	COMPILE_PDB_NAME "" # use default
	COMPILE_PDB_OUTPUT_DIRECTORY "$<TARGET_FILE_DIR:LNK4206>")

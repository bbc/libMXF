add_executable(MXFDump
    MXFDump.cpp
)

if(UNIX)
    # Ignore unsed functions warnings
    target_compile_options(MXFDump PRIVATE -Wno-unused-function)
endif()

install(TARGETS MXFDump DESTINATION bin)
add_executable(MXFDump
    MXFDump.cpp
)

if(UNIX)
    # Ignore unsed functions warnings
    target_compile_options(MXFDump PRIVATE -Wno-unused-function)
endif()

# APPLE requires link to CoreServices library for functions such as FSOpenFork
if(APPLE)
    # Use target_link_libraries to add linker options. This is equivalent to
    # target_link_options which was added in cmake 3.13
    target_link_libraries(MXFDump -framework CoreServices)
endif()

install(TARGETS MXFDump DESTINATION bin)
add_executable(archive_mxf_info
    archive_mxf_info.c
)

target_link_libraries(archive_mxf_info
    MXF
)

install(TARGETS archive_mxf_info DESTINATION bin)

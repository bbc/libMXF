add_executable(extract_avid_extensions
    extract_avid_extensions.c
)
target_include_directories(extract_avid_extensions PUBLIC
    ${PROJECT_SOURCE_DIR}
)
target_link_libraries(extract_avid_extensions MXF)

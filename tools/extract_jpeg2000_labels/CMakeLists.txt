add_executable(extract_jpeg2000_labels
    extract_jpeg2000_labels.c
)
target_include_directories(extract_jpeg2000_labels PUBLIC
    ${PROJECT_SOURCE_DIR}
)
target_link_libraries(extract_jpeg2000_labels MXF)

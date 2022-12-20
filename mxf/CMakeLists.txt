add_library(MXF
    mxf_app.c
    mxf_avid.c
    mxf_avid_dictionary.c
    mxf_avid_dictionary_data.h
    mxf_avid_metadictionary.c
    mxf_avid_metadictionary_data.h
    mxf_cache_file.c
    mxf_data_model.c
    mxf_essence_container.c
    mxf_file.c
    mxf_header_metadata.c
    mxf_index_table.c
    mxf_labels_and_keys.c
    mxf_list.c
    mxf_logging.c
    mxf_memory_file.c
    mxf_page_file.c
    mxf_partition.c
    mxf_primer.c
    mxf_rw_intl_file.c
    mxf_stream_file.c
    mxf_tree.c
    mxf_utils.c
    mxf_uu_metadata.c
    mxf_version.c

    $<$<BOOL:${WIN32}>:mxf_win32_file.c>
    $<$<BOOL:${WIN32}>:mxf_win32_mmap.c>
)

set(MXF_headers
    mxf.h
    mxf_app.h
    mxf_app_extensions_data_model.h
    mxf_app_types.h
    mxf_avid.h
    mxf_avid_dictionary.h
    mxf_avid_extensions_data_model.h
    mxf_avid_labels_and_keys.h
    mxf_avid_metadictionary.h
    mxf_baseline_data_model.h
    mxf_cache_file.h
    mxf_data_model.h
    mxf_essence_container.h
    mxf_extensions_data_model.h
    mxf_file.h
    mxf_header_metadata.h
    mxf_index_table.h
    mxf_labels_and_keys.h
    mxf_list.h
    mxf_logging.h
    mxf_macros.h
    mxf_memory_file.h
    mxf_page_file.h
    mxf_partition.h
    mxf_primer.h
    mxf_rw_intl_file.h
    mxf_stream_file.h
    mxf_tree.h
    mxf_types.h
    mxf_utils.h
    mxf_uu_metadata.h
    mxf_version.h

    $<$<BOOL:${WIN32}>:mxf_win32_file.h>
    $<$<BOOL:${WIN32}>:mxf_win32_mmap.h>
)

target_include_directories(MXF PUBLIC
    ${PROJECT_SOURCE_DIR}
)

# Add path to header files if not included in MSVC distribution
if(MSVC)
    include(CheckIncludeFile)

    CHECK_INCLUDE_FILE(inttypes.h have_inttypes_header)
    if(NOT have_inttypes_header)
        target_include_directories(bmx PUBLIC
            ${PROJECT_SOURCE_DIR}/msvc_build/include/inttypes
        )
    endif()
endif()

target_include_directories(MXF PRIVATE
    ${PROJECT_BINARY_DIR}
)
target_compile_definitions(MXF PRIVATE
    HAVE_CONFIG_H
)

if(CMAKE_C_COMPILER_ID STREQUAL "GNU")
    # Use target_link_libraries to add linker options. This is equivalent to
    # target_link_options which was added in cmake 3.13
    target_link_libraries(MXF PRIVATE -Wl,--no-undefined)
endif()

target_link_libraries(MXF PRIVATE
    ${uuid_link_lib}
)

# Add the git version tracking library code
include("${PROJECT_SOURCE_DIR}/cmake/git_version.cmake")
target_link_libraries(MXF PRIVATE
    cmake_git_version_tracking
)

set_target_properties(MXF PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
)

install(TARGETS MXF LIBRARY DESTINATION lib)
install(FILES ${MXF_headers} DESTINATION include/mxf)

if(UNIX)
    configure_file(../${PROJECT_NAME}.pc.in ${PROJECT_NAME}.pc @ONLY)
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}.pc DESTINATION lib/pkgconfig)
endif()
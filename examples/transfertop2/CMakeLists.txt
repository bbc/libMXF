add_library(avid_mxf_to_p2
    avid_mxf_to_p2.c
    xml_writer.c
)

target_link_libraries(avid_mxf_to_p2
    MXF
)


if(LIBMXF_BUILD_TESTING AND (NOT DEFINED BUILD_TESTING OR BUILD_TESTING))
    add_executable(test_avid_mxf_to_p2
        test_avid_mxf_to_p2.c
    )
    target_link_libraries(test_avid_mxf_to_p2
        avid_mxf_to_p2
        MXF
    )
endif()

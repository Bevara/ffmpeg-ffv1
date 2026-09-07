cmake_minimum_required(VERSION 3.7)
project(Filters)

include(filters.cmake)

add_definitions(-fpic)

SET(FF_SRC_FFV1
        ${CMAKE_CURRENT_SOURCE_DIR}/ff_dec-ffv1.c
        ${CMAKE_CURRENT_SOURCE_DIR}/ff_common.c
)

SET(FF_LIB_FFV1
        ${CMAKE_CURRENT_SOURCE_DIR}/lib/libavcodec.a
        ${CMAKE_CURRENT_SOURCE_DIR}/lib/libavutil.a
        ${CMAKE_CURRENT_SOURCE_DIR}/lib/libavformat.a
        ${CMAKE_CURRENT_SOURCE_DIR}/lib/libavfilter.a
        ${CMAKE_CURRENT_SOURCE_DIR}/lib/libswscale.a
)

SET(FF_INCLUDE_FFV1
        ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_filter(ffmpeg-ffv1
        "${FF_SRC_FFV1}"
        "${FF_LIB_FFV1}"
        []
        GPAC_HAS_FFMPEG
        "${FF_INCLUDE_FFV1}"
        ""
        "1")

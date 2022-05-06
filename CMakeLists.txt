cmake_minimum_required(VERSION 3.22)
project(tumble)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

find_package(TIFF)
find_package(NetPBM)
find_package(JPEG)

add_executable(bitblt_table_gen bitblt_table_gen.c)
add_executable(g4_table_gen g4_table_gen.c)

add_executable(tumble
    bitblt_g4.c
    bitblt.c
    bitblt_tables.c
    bitblt_tables.h
    g4_tables.c
    g4_tables.h    
    pdf_bookmark.c
    pdf_g4.c
    pdf_jpeg.c
    pdf_name_tree.c
    pdf_page_label.c
    pdf_png.c
    pdf_prim.c
    pdf_text.c
    pdf_util.c
    pdf.c
    semantics.c
    tumble_blank.c
    tumble_input.c
    tumble_jpeg.c
    tumble_pbm.c
    tumble_png.c
    tumble_tiff.c
    tumble.c)

add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/bitblt_tables.c ${CMAKE_CURRENT_SOURCE_DIR}/bitblt_tables.h
    COMMAND bitblt_table_gen -h >${CMAKE_CURRENT_SOURCE_DIR}/bitblt_tables.h
    COMMAND bitblt_table_gen -c >${CMAKE_CURRENT_SOURCE_DIR}/bitblt_tables.c    
    )

add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/g4_tables.c ${CMAKE_CURRENT_SOURCE_DIR}/g4_tables.h
    COMMAND g4_table_gen -h >${CMAKE_CURRENT_SOURCE_DIR}/g4_tables.h
    COMMAND g4_table_gen -c >${CMAKE_CURRENT_SOURCE_DIR}/g4_tables.c    
    )

target_include_directories(tumble PUBLIC ${TIFF_INCLUDE_DIRS} ${JPEG_INCLUDE_DIRS})
target_link_libraries(tumble ${TIFF_LIBRARIES} ${JPEG_LIBRARIES} ${NETPBM_LIBRARIES})

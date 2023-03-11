cmake_minimum_required(VERSION 3.0)

include(tacklelib/Project)
include(tacklelib/_3dparty/Qt)

project("qcustomplot")

set(PROJECT_LIB_NAME qcustomplot)

tkl_check_var(REQUIRED PATH   QCUSTOMPLOT_BRANCH_NAME) # ex: `2_1_1_release`

set(PROJECT_EXTERNALS_DIR "_externals")
set(QCUSTOMPLOT_EXTERNAL_SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}/${PROJECT_EXTERNALS_DIR}/${QCUSTOMPLOT_BRANCH_NAME}")


file(GLOB_RECURSE all_headers ${QCUSTOMPLOT_EXTERNAL_SOURCE_DIR}/*.h*)
file(GLOB_RECURSE all_sources ${QCUSTOMPLOT_EXTERNAL_SOURCE_DIR}/*.c*)

# if Qt enabled
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

add_library(${PROJECT_LIB_NAME} ${all_headers};${all_sources})

tkl_initialize_library_target_defaults(${PROJECT_LIB_NAME} "anybit")

tkl_source_groups_from_dir_list("Header Files" FILES ${QCUSTOMPLOT_EXTERNAL_SOURCE_DIR} *.h*)
tkl_source_groups_from_dir_list("Source Files" FILES ${QCUSTOMPLOT_EXTERNAL_SOURCE_DIR} *.c*)

tkl_source_groups_from_dir_list("Generated Files" FILES ${PROJECT_ROOT}/_out/build qrc_*.c*)
tkl_source_groups_from_dir_list("Generated Files" FILES ${PROJECT_ROOT}/_out/build mocs_*.c*)

target_include_directories(${PROJECT_LIB_NAME}
  PUBLIC
    ${QCUSTOMPLOT_EXTERNAL_SOURCE_DIR}
)

link_qt_components(${PROJECT_LIB_NAME} PUBLIC PUBLIC QT_LIBRARIES QT_COMPONENTS)

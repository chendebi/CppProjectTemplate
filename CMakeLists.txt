cmake_minimum_required(VERSION 3.2)

project(YOUR_PROJECT_NAME)

set(CMAKE_CXX_STANDARD 17)

string(COMPARE EQUAL "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}" LIBRARY_STANDALONE)

option(LIBRARY_BUILD_TESTS "Build the MineLibraryCPP test programs" ${LIBRARY_STANDALONE})

if(${LIBRARY_STANDALONE})
    add_subdirectory(test)
endif(${LIBRARY_STANDALONE})

function(make_source_group BASE_DIR)
    message(STATUS "Add source files in base dir ${BASE_DIR}")
    foreach(fileItem ${ARGN})
        # 先获取文件的绝对路径
        get_filename_component(ABS_FILE_PATH "${fileItem}" ABSOLUTE "${BASE_DIR}")
        # 获取文件的文件夹路径
        get_filename_component(FILE_PATH "${ABS_FILE_PATH}" PATH)
        # 替换基础路径得到相对路径
        string(REPLACE "${BASE_DIR}" "" GROUP "${FILE_PATH}")
        # 替换windows路径分隔符
        string(REPLACE "." "\\" GROUP "${GROUP}")
        # 添加到文件夹group
        set(GROUP "${GROUP}")
        source_group("${GROUP}" FILES "${fileItem}")
        message(STATUS "file: ${ABS_FILE_PATH} added to group ${GROUP}")
    endforeach()
endfunction()
file(GLOB_RECURSE project_source_files "./Source/**.cpp" "./Include/**.h")
make_source_group(${CMAKE_CURRENT_SOURCE_DIR} "${project_source_files}")

add_library(${PROJECT_NAME} STATIC ${project_source_files})

target_include_directories(
    ${PROJECT_NAME} 
    PUBLIC 
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

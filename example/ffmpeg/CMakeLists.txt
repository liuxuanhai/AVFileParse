cmake_minimum_required(VERSION 3.15)

# 获取当前文件夹名
STRING(REGEX REPLACE ".*/(.*)" "\\1" CURRENT_FOLDER ${CMAKE_CURRENT_SOURCE_DIR})

project(${CURRENT_FOLDER})

# 分别设置了Debug版本和Release版本可执行文件的输出目录(经测试,windows环境有效)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/../bin/debug)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/../bin/release) 

# 分别设置了Debug版本和Release版本库文件的输出目录(经测试,windows环境有效)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/../lib/debug)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/../lib/release)

# 包含路径
include_directories(${PROJECT_NAME} ${PROJECT_SOURCE_DIR}/../../ffmpeg/include)

# 链接库目录
link_directories(${PROJECT_NAME} ${PROJECT_SOURCE_DIR}/../../ffmpeg/lib)

# 添加源码文件到项目
FILE(GLOB header ./*.h ../aac/*.h)
aux_source_directory(. SRCS)
add_executable(${PROJECT_NAME} ${header} ${SRCS})

# 链接库
target_link_libraries(${PROJECT_NAME} avformat.lib avcodec.lib avutil.lib)

# 使用DEBUG_POSTFIX指定输出文件结尾对executable target(可执行文件)无效
#set (CMAKE_DEBUG_POSTFIX d)
#set_target_properties (${PROJECT_NAME} PROPERTIES DEBUG_POSTFIX _d)
set_property(TARGET ${PROJECT_NAME} PROPERTY DEBUG_POSTFIX _d)

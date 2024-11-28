cmake_minimum_required(VERSION 3.31)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(Copilot)

# 查找所需的包
find_package(unofficial-webview2 CONFIG REQUIRED)
find_path(SIMPLEINI_INCLUDE_DIRS "ConvertUTF.c")

# 包含项目里自己写的include目录
include_directories(${CMAKE_SOURCE_DIR}/include)

# 所有cpp文件
file(GLOB_RECURSE CPP_SOURCES "src/*.cpp")

# 创建可执行文件
add_executable(${PROJECT_NAME} WIN32 ${CPP_SOURCES} ./assets/resource.rc)

# 第三方库的include目录：simpleini
target_include_directories(${PROJECT_NAME} PRIVATE ${SIMPLEINI_INCLUDE_DIRS})

# 链接所需的库：webview2、fmt
target_link_libraries(${PROJECT_NAME} PRIVATE unofficial::webview2::webview2)

# 在 MinGW 下设置编译选项
if(MINGW)
  add_compile_options(-finput-charset=UTF-8 -fexec-charset=UTF-8)
endif()

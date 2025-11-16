cmake_minimum_required(VERSION 3.12)
project(
  demo
  VERSION 0.0.1
  LANGUAGES C CXX)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
set(ENV{VULKAN_SDK} ${CMAKE_SOURCE_DIR}/src/middlewares/vk_sdks)

# 全局包含目录
include_directories(${CMAKE_SOURCE_DIR}/include ${CMAKE_SOURCE_DIR}/src/easyvk ${CMAKE_SOURCE_DIR}/src/middlewares/stb_image)

# 全局包含库目录(指定库的搜索路径)
link_directories(${CMAKE_SOURCE_DIR}/lib)

#第三方库
add_subdirectory(${CMAKE_SOURCE_DIR}/src/middlewares/glfw)

# 源文件
file(GLOB glad_src ${CMAKE_SOURCE_DIR}/src/middlewares/glad/glad.c)
file(GLOB main_src ${CMAKE_SOURCE_DIR}/src/main.cpp)
file(GLOB test_src ${CMAKE_SOURCE_DIR}/src/test.cpp)

# 找到库
find_package(Vulkan REQUIRED)

# 可执行文件
add_executable(main_src ${main_src})
add_executable(test_src ${test_src})

# 链接头文件
target_include_directories(main_src PUBLIC ${Vulkan_INCLUDE_DIRS})
target_include_directories(test_src PUBLIC ${Vulkan_INCLUDE_DIRS})

# # 链接库目录
# target_link_directories(main_src PRIVATE ${CMAKE_SOURCE_DIR}/lib/glfw)

# 链接库
target_link_libraries(main_src PRIVATE glfw Vulkan::Vulkan)
target_link_libraries(test_src PRIVATE glfw Vulkan::Vulkan)

# CMakeList.txt : VulkanTemplateProject の CMake プロジェクト。ソースを含めて、次を定義します:
# プロジェクト専用ロジックはこちらです。

# Visual Studio 2022

# CMakeのバージョンは一応公式チュートリアルと合わせた（2025/7/16）
# https://github.com/KhronosGroup/Vulkan-Tutorial/blob/main/attachments/CMakeLists.txt
cmake_minimum_required (VERSION 3.29)

# C++の標準規格は公式によると20を想定
# https://docs.vulkan.org/tutorial/latest/00_Introduction.html#_differences
set (CMAKE_CXX_STANDARD 20)


# サポートされている場合は、MSVC コンパイラのホット リロードを有効にします。
# (Visual Studioが自動で生成した)
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

# ${PROJECT_NAME}の定義
project ("VulkanTemplateProject")


# ソースをこのプロジェクトの実行可能ファイルに追加
add_executable (${PROJECT_NAME} "HelloTriangleApplication.cpp")







# パッケージインストール時／ビルド時にfind_packageとtarget_link_librariesの形式が出力されるので、
# それをコピペすること

# find_packageのときは 3 があるが、target_link_librariesのときはないので注意
find_package(glfw3 CONFIG REQUIRED)
target_link_libraries(${PROJECT_NAME} PRIVATE glfw)


find_package(glm CONFIG REQUIRED)
target_link_libraries(${PROJECT_NAME} PRIVATE glm::glm)


# 「エラー E2357	指定子と非集約型 "vk::ApplicationInfo" は同時に使用できません」の対策
# globally define VULKAN_HPP_NO_STRUCT_CONSTRUCTORS to have designated initializers for the vk-structs
# https://github.com/KhronosGroup/Vulkan-Samples/blob/main/CMakeLists.txt
add_compile_definitions(VULKAN_HPP_NO_STRUCT_CONSTRUCTORS)

# Vulkan SDKはvcpkg外からのリンクになるので、インストールディレクトリを指定しなければいけない
target_include_directories(${PROJECT_NAME} PRIVATE $ENV{VULKAN_SDK}/Include)


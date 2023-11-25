cmake_minimum_required(VERSION 3.24)
project(imgui CXX)

# Adapted from
# vcpkg/ports/imgui/CMakeLists.txt

find_package(Vulkan REQUIRED)

add_library(${PROJECT_NAME} "")
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

set_target_properties(${PROJECT_NAME}
    PROPERTIES
        CXX_STANDARD 11
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
        DEBUG_POSTFIX d
)

target_sources(${PROJECT_NAME}
    PUBLIC
        FILE_SET common
        TYPE HEADERS
        BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}
        FILES
            imconfig.h
            imgui_internal.h
            imgui.h
            imstb_rectpack.h
            imstb_textedit.h
            imstb_truetype.h

        FILE_SET impl
        TYPE HEADERS
        BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/backends
        FILES
            backends/imgui_impl_glfw.h
            backends/imgui_impl_vulkan.h

    PRIVATE
        imgui.cpp
        imgui_demo.cpp
        imgui_draw.cpp
        imgui_tables.cpp
        imgui_widgets.cpp
        misc/cpp/imgui_stdlib.cpp
        backends/imgui_impl_glfw.cpp
        backends/imgui_impl_vulkan.cpp
)

target_link_libraries(${PROJECT_NAME}
    PUBLIC
        glfw
        Vulkan::Vulkan
)

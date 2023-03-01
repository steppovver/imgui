cmake_minimum_required(VERSION 3.0.0)
project(imgui VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

if(MSVC)
        add_definitions(-DUNICODE)
        add_definitions(-D_UNICODE)
endif()

# Ensure ImGui integration file uses glad loader
add_definitions(-DIMGUI_IMPL_OPENGL_LOADER_GLAD)

option(IMFRAME_BUILD_STB "Build stb library" ON)

# Set some specific glfw options
option(GLFW_BUILD_EXAMPLES "Build the GLFW example programs" OFF)
option(GLFW_BUILD_TESTS "Build the GLFW test programs" OFF)
option(GLFW_BUILD_DOCS "Build the GLFW documentation" OFF)
option(GLFW_INSTALL "Generate installation target" OFF)

SET(PUBLIC_HEADERS
    imgui.h
    backends/imgui_impl_glfw.h
    backends/imgui_impl_opengl3.h
)

SET(SOURCES
    imgui.cpp
    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.cpp

    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
)

add_library(${PROJECT_NAME} STATIC
    ${PUBLIC_HEADERS} ${SOURCES}
)

# Set compiler options
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE /W4 /WX)
else()
    target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -pedantic -Werror)
endif()

target_include_directories(${PROJECT_NAME} PUBLIC ..)
target_include_directories(${PROJECT_NAME} PUBLIC . backends)
target_link_libraries(${PROJECT_NAME} PRIVATE glad glfw)

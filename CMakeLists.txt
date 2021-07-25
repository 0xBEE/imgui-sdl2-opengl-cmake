cmake_minimum_required(VERSION 3.19)
project(PROJECT_NAME)

set(CMAKE_CXX_STANDARD 20)
find_package(SDL2 REQUIRED)
find_package(OpenGL REQUIRED)

add_library(imgui
        # Main Imgui files
        ${CMAKE_SOURCE_DIR}/imgui/imgui.cpp ${CMAKE_SOURCE_DIR}/imgui/imgui_draw.cpp
        ${CMAKE_SOURCE_DIR}/imgui/imgui_tables.cpp ${CMAKE_SOURCE_DIR}/imgui/imgui_widgets.cpp
        ${CMAKE_SOURCE_DIR}/imgui/imgui_demo.cpp
        # SDL2+OpenGL-specific files
        ${CMAKE_SOURCE_DIR}/imgui/backends/imgui_impl_sdl.cpp
        ${CMAKE_SOURCE_DIR}/imgui/backends/imgui_impl_opengl3.cpp)

add_executable(${CMAKE_PROJECT_NAME} main.cpp)

target_link_libraries(${CMAKE_PROJECT_NAME} ${SDL2_LIBRARIES})
target_link_libraries(${CMAKE_PROJECT_NAME} ${OPENGL_LIBRARIES})
target_link_libraries(${CMAKE_PROJECT_NAME} imgui)

target_include_directories(${CMAKE_PROJECT_NAME} PUBLIC ${SDL2_INCLUDE_DIRS})
target_include_directories(${CMAKE_PROJECT_NAME} PUBLIC ${OPENGL_INCLUDE_DIR})
# Make SDL2 available to Imgui
target_include_directories(imgui PUBLIC ${SDL2_INCLUDE_DIRS})
# imgui/backends/ contains the SDL implementation
target_include_directories(imgui PUBLIC ${CMAKE_SOURCE_DIR}/imgui/ ${CMAKE_SOURCE_DIR}/imgui/backends/)
# Configure Imgui to use OpengGL through SDL2
target_compile_definitions(imgui PUBLIC IMGUI_IMPL_OPENGL_LOADER_CUSTOM=<SDL_opengl.h>  GL_GLEXT_PROTOTYPES=1)
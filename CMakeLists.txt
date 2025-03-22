cmake_minimum_required(VERSION 3.15)
set(CMAKE_C_STANDARD 99)
project(raygui C)

find_package(raylib REQUIRED)

add_library(${PROJECT_NAME} STATIC
    src/raygui.c
)
target_include_directories(${PROJECT_NAME} PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/icons
)
target_link_libraries(${PROJECT_NAME} PUBLIC
    raylib::raylib
)
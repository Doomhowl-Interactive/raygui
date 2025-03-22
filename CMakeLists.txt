cmake_minimum_required(VERSION 3.11)
set(CMAKE_C_STANDARD 99)
project(raygui C)

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/icons
)
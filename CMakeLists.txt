cmake_minimum_required(VERSION 3.15)
project(raygui C)
set(CMAKE_C_STANDARD 99)

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
option(NDK_PORTS "NDK build target" OFF)

set(RAYGUI_SRC src/raygui.c)

if (NOT NDK_PORTS)
    find_package(raylib REQUIRED)
endif ()

add_library(${PROJECT_NAME}
        ${RAYGUI_SRC}
)

target_include_directories(${PROJECT_NAME} PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/icons>
        $<INSTALL_INTERFACE:include>
)

if (NDK_PORTS)
    target_link_directories(${PROJECT_NAME} PUBLIC
            ${RAYLIB_LIBRARIES}
    )
    target_include_directories(${PROJECT_NAME} PUBLIC
            ${RAYLIB_INCLUDE_DIR}
    )
    target_link_libraries(${PROJECT_NAME} PUBLIC
            ${RAYLIB_LIBRARY}
    )
else ()
    target_link_libraries(${PROJECT_NAME} PUBLIC
            raylib::raylib
    )
endif ()

install(TARGETS ${PROJECT_NAME}
        EXPORT rayguiTargets
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include
)

install(DIRECTORY src/ DESTINATION include
        FILES_MATCHING PATTERN "*.h"
)

install(EXPORT rayguiTargets
        FILE rayguiTargets.cmake
        NAMESPACE raygui::
        DESTINATION lib/cmake/raygui
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/rayguiConfigVersion.cmake"
        VERSION 1.0.0
        COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
        "${CMAKE_CURRENT_SOURCE_DIR}/cmake/rayguiConfig.cmake.in"
        "${CMAKE_CURRENT_BINARY_DIR}/rayguiConfig.cmake"
        INSTALL_DESTINATION lib/cmake/raygui
)

install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/rayguiConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/rayguiConfigVersion.cmake"
        DESTINATION lib/cmake/raygui
)
cmake_minimum_required(VERSION 3.9)

project(foo VERSION 1.0 DESCRIPTION "CPP Lib template")

set(DEFAULT_BUILD_TYPE "Release")
option(FOO_STATIC_LIB "Build static library" ON)

if(FOO_STATIC_LIB)
    add_library(${PROJECT_NAME} STATIC src/foo.cpp)
else()
    add_library(${PROJECT_NAME} SHARED src/foo.cpp)
endif()

set_target_properties(${PROJECT_NAME} PROPERTIES VERSION ${PROJECT_VERSION})

set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER include/foo.h)

target_include_directories(${PROJECT_NAME} PRIVATE include)
target_include_directories(${PROJECT_NAME} PRIVATE src)

include(GNUInstallDirs)

install(TARGETS ${PROJECT_NAME} 
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

configure_file(foo.pc.in foo.pc @ONLY)

install(FILES ${CMAKE_BINARY_DIR}/foo.pc DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)
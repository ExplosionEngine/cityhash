cmake_minimum_required(VERSION 3.20)
project(cityhash)

set(CMAKE_CXX_STANDARD 11)

file(GLOB SOURCES src/*.cc)
add_library(city ${SOURCES})
target_include_directories(city PRIVATE ${CMAKE_BINARY_DIR})
target_include_directories(city PUBLIC src)
if (${MSVC})
    target_compile_options(city PUBLIC /MD$<$<CONFIG:Debug>:d>)
endif()

add_custom_target(
    default-config
    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_SOURCE_DIR}/config.h.in ${CMAKE_BINARY_DIR}/config.h
)
add_dependencies(city default-config)

file(GLOB HEADERS src/*.h)
install(
    TARGETS city
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
INSTALL(FILES ${HEADERS} DESTINATION include)

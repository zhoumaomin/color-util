cmake_minimum_required(VERSION 3.1)

project(color-util CXX)
set(CMAKE_CXX_STANDARD 11)

find_package(Eigen3 REQUIRED)

file(GLOB HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/color-util/*.hpp)

add_library(color-util INTERFACE)
target_sources(color-util INTERFACE ${HEADERS})
target_include_directories(color-util INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(color-util INTERFACE Eigen3::Eigen)

install(FILES ${HEADERS} DESTINATION include/color-util)

option(COLORUTIL_BUILD_TESTS "Build tests" OFF)

if(COLORUTIL_BUILD_TESTS)
	add_subdirectory(tests/conversions)
	add_subdirectory(tests/CIEDE2000)

	enable_testing()
	add_test(NAME test-conversions COMMAND $<TARGET_FILE:test-conversions> ${CMAKE_BINARY_DIR})
	add_test(NAME test-CIEDE2000 COMMAND $<TARGET_FILE:test-CIEDE2000> ${CMAKE_BINARY_DIR})
endif()
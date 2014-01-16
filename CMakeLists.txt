cmake_minimum_required(VERSION 2.6.3)
project(osg)

set(CMAKE_BUILD_TYPE "Release")

include_directories(${PROJECT_SOURCE_DIR}/include)

# set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# Add executables
set(OSG_LIBRARIES osg osgViewer osgGA)
link_libraries(${OSG_LIBRARIES})

file(GLOB lib_source "src/*.cpp" "include/*.h" "graphics/*.cpp")
list(SORT lib_source)

file(GLOB unit_tests_source "test/*.cpp")
LIST(SORT unit_tests_source)

message(STATUS "\n -- UNIT TEST: ")
add_custom_target(check COMMAND ${CMAKE_CTEST_COMMAND})
foreach(utest_src_file ${unit_tests_source})
    get_filename_component(test_base ${utest_src_file} NAME_WE)
    message(STATUS "Adding test ${test_base}")
    add_executable(${test_base} ${utest_src_file})
    target_link_libraries(${test_base} ${PROJECT_NAME})
    add_test(${test_base} ${EXECUTABLE_OUTPUT_PATH}/${test_base})
    add_custom_target(${test_base}.run ${test_base} ${ARGN})
    add_dependencies(check ${test_base})
endforeach(utest_src_file)

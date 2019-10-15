cmake_minimum_required(VERSION 3.10)
project(UnitTest++)

option(UTPP_INCLUDE_TESTS_IN_BUILD
    "Set this to OFF if you do not wish to automatically build or run unit tests as part of the default cmake --build"
    OFF)

set(LIB_SUFFIX "" CACHE STRING "Identifier to add to end of lib directory name e.g. 64 for lib64")

#######################################################################
# Set up compiler flags
#######################################################################
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
endif()
if(CMAKE_BUILD_TYPE STREQUAL "Release")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -s")
endif()
string(CONCAT CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wextra -pedantic "
    "-Winit-self -Wmissing-declarations -Wmissing-include-dirs "
    "-Wundef -Wredundant-decls -Wfloat-equal -Wmain -Wcast-align "
    "-Wswitch-enum")

# get the main sources
file(GLOB headers_ RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} UnitTest++/src/*.h)
file(GLOB sources_ RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} UnitTest++/src/*.cpp)
source_group("" FILES ${headers_} ${sources_})

# get platform specific sources
set(platformDir_ Posix)

file(GLOB platformHeaders_ RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    UnitTest++/src/${platformDir_}/*.h)
file(GLOB platformSources_ RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    UnitTest++/src/${platformDir_}/*.cpp)
source_group(${platformDir_} FILES ${platformHeaders_} ${platformSources_})

# create the lib
add_library(UnitTest++ STATIC ${headers_} ${sources_} ${platformHeaders_}
    ${platformSources_})

set_target_properties(UnitTest++ PROPERTIES OUTPUT_NAME UnitTest++)

# build the test runner
file(GLOB TEST_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    UnitTest++/src/tests/*.cpp UnitTest++/src/tests/*.h)
source_group("" FILES ${TEST_SRCS})
add_executable(TestUnitTest++ ${TEST_SRCS})
include_directories(UnitTest++/src)

set_target_properties(TestUnitTest++ PROPERTIES OUTPUT_NAME TestUnitTest++)
target_link_libraries(TestUnitTest++ UnitTest++)

# run unit tests as post build step
add_custom_command(TARGET TestUnitTest++
    POST_BUILD COMMAND TestUnitTest++
    COMMENT "Running unit tests")

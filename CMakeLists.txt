cmake_minimum_required(VERSION 3.4)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(bank)
set(BANK_VERSION_MAJOR 0)
set(BANK_VERSION_MINOR 1)
set(BANK_VERSION_PATCH 0)
set(BANK_VERSION_TWEAK 0)
set(BANK_VERSION ${BANK_VERSION_MAJOR}.${BANK_VERSION_MINOR}.${BANK_VERSION_PATCH}.${BANK_VERSION_TWEAK})
set(BANK_VERSION_STRING "v${BANK_VERSION}")

add_subdirectory(third-party/gtest)
enable_testing()

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/banking)

add_library(account STATIC ${CMAKE_CURRENT_SOURCE_DIR}/banking/Account.cpp)
add_library(transaction STATIC ${CMAKE_CURRENT_SOURCE_DIR}/banking/Transaction.cpp)

include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR})
include_directories(${gmock_SOURCE_DIR}/include ${gmock_SOURCE_DIR}) 

target_link_libraries(transaction account)
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/third-party/CMake-codecov/cmake" ${CMAKE_MODULE_PATH})

file(GLOB SRC tests/*.cpp)

add_executable(t1st ${SRC})
target_link_libraries(t1st gtest gtest_main gmock account transaction)
add_test(NAME t1st COMMAND t1st)

set(ENABLE_COVERAGE ON CACHE BOOL "Enable coverage build." FORCE)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
find_package(codecov)
add_coverage(t1st)
list(APPEND LCOV_REMOVE_PATTERNS "'${PROJECT_SOURCE_DIR}/tests/*'")
coverage_evaluate()
include(CPackConfig.cmake)
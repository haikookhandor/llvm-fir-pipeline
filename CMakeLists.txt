cmake_minimum_required(VERSION 3.16)
project(FIRCompiler LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED YES)
set(CMAKE_CXX_EXTENSIONS NO)

# GoogleTest via FetchContent
include(FetchContent)
FetchContent_Declare(
    googletest
    URL https://github.com/google/googletest/archive/refs/tags/release-1.12.1.zip
)
# For Windows: prevent overriding the parent project's compiler settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

enable_testing()

# Library target for FIR compiler
add_library(FIRlib STATIC
    src/dummy.cpp
)
target_include_directories(FIRlib PUBLIC ${PROJECT_SOURCE_DIR}/include)
# (Sources will be added in later commits as the project evolves)

# Test target
add_executable(runTests tests/TestBasic.cpp)
target_link_libraries(runTests PRIVATE FIRlib GTest::gtest_main)

include(GoogleTest)
gtest_discover_tests(runTests)

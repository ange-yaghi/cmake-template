cmake_minimum_required(VERSION 3.10)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(GLOBAL PROPERTY PREDEFINED_TARGETS_FOLDER "cmake")

set(ProjectName "cmake-template")
set(ProjectNamespace "atg_tmpl")

project("${ProjectName}")

set(CMAKE_CXX_STANDARD 11)

# =========================================================
# gtest

include(FetchContent)
FetchContent_Declare(
    googletest
    URL
    https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)

set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

set_property(TARGET gmock PROPERTY FOLDER "gtest")
set_property(TARGET gmock_main PROPERTY FOLDER "gtest")
set_property(TARGET gtest PROPERTY FOLDER "gtest")
set_property(TARGET gtest_main PROPERTY FOLDER "gtest")

# =========================================================

add_library("${ProjectName}" STATIC
    # Source files
    src/information.cpp

    # Header files
    include/information.h
)

target_compile_definitions("${ProjectName}" PRIVATE PROJECT_NAMESPACE=${ProjectNamespace})

add_executable("${ProjectName}-main"
    # Source files
    src/main.cpp

    # Header files
)

# gtest

enable_testing()

add_executable("${ProjectName}-test"
    # Source files
    test/sanity_test.cpp

    # Head files
)

target_link_libraries("${ProjectName}-test"
    gtest_main
    "${ProjectName}"
)

include(GoogleTest)
gtest_discover_tests("${ProjectName}-test")

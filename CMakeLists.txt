cmake_minimum_required(VERSION 3.20)
project(modern_cpp_edu CXX)

# Check if compiler supports various C++ standards
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++26" COMPILER_SUPPORTS_CXX26)
CHECK_CXX_COMPILER_FLAG("-std=c++2c" COMPILER_SUPPORTS_CXX2C)
CHECK_CXX_COMPILER_FLAG("-std=c++23" COMPILER_SUPPORTS_CXX23)
CHECK_CXX_COMPILER_FLAG("-std=c++2b" COMPILER_SUPPORTS_CXX2B)
CHECK_CXX_COMPILER_FLAG("-std=c++20" COMPILER_SUPPORTS_CXX20)

# Set the highest available C++ standard
if(COMPILER_SUPPORTS_CXX26)
    set(CMAKE_CXX_STANDARD 26)
    message(STATUS "Using C++26")
elseif(COMPILER_SUPPORTS_CXX2C)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++2c")
    message(STATUS "Using C++2c (experimental C++26)")
elseif(COMPILER_SUPPORTS_CXX23)
    set(CMAKE_CXX_STANDARD 23)
    message(STATUS "Using C++23")
elseif(COMPILER_SUPPORTS_CXX2B)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++2b")
    message(STATUS "Using C++2b (experimental C++23)")
elseif(COMPILER_SUPPORTS_CXX20)
    set(CMAKE_CXX_STANDARD 20)
    message(STATUS "Using C++20")
else()
    set(CMAKE_CXX_STANDARD 17)
    message(WARNING "C++20 not supported, falling back to C++17")
endif()

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Common compiler flags
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    add_compile_options(-Wall -Wextra -pedantic)
    add_compile_options(-pthread)
    add_link_options(-pthread)
    
    # Enable coroutines
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        add_compile_options(-fcoroutines)
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
        add_compile_options(-stdlib=libc++ -fcoroutines-ts)
    endif()
endif()

# Set default build type if not specified
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Build type" FORCE)
endif()

# Output directory for executables
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Add subdirectories for each section
add_subdirectory(josuttis_cppstd11)
add_subdirectory(josuttis_cppstd20)
add_subdirectory(rainer_grimm_modernes_c++)
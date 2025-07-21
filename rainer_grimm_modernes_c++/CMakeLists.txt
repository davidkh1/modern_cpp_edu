cmake_minimum_required(VERSION 3.20)
project(rainer_grimm_modernes_cpp CXX)

# Source directory
set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

# Find all .cpp files in the source directory
file(GLOB CPP_FILES ${SRC_DIR}/*.cpp)

# Create an executable for each .cpp file
foreach(CPP_FILE ${CPP_FILES})
    get_filename_component(EXEC_NAME ${CPP_FILE} NAME_WE)
    set(TARGET_NAME "grimm_${EXEC_NAME}")
    
    add_executable(${TARGET_NAME} ${CPP_FILE})
    
    # Create output directory
    file(MAKE_DIRECTORY "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/rainer_grimm_modernes_c++")
    
    set_target_properties(${TARGET_NAME} PROPERTIES
        OUTPUT_NAME "${EXEC_NAME}"
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/rainer_grimm_modernes_c++"
    )
    
    # All examples in this directory are concurrency-related
    target_compile_options(${TARGET_NAME} PRIVATE -pthread)
    target_link_libraries(${TARGET_NAME} pthread)
    
    # Enable coroutines if needed
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        target_compile_options(${TARGET_NAME} PRIVATE -fcoroutines)
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
        target_compile_options(${TARGET_NAME} PRIVATE -fcoroutines-ts -stdlib=libc++)
        target_link_libraries(${TARGET_NAME} c++)
    endif()
endforeach()

message(STATUS "Configured Rainer Grimm Modern C++ examples")
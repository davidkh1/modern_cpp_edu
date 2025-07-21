cmake_minimum_required(VERSION 3.20)
project(josuttis_cppstd11 CXX)

# Source directory
set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

# Add all subdirectories to include path
include_directories(${SRC_DIR})
include_directories(${SRC_DIR}/util)
include_directories(${SRC_DIR}/stl)

# Get all subdirectories
file(GLOB SUBDIRS RELATIVE ${SRC_DIR} ${SRC_DIR}/*)

# Process each subdirectory
foreach(SUBDIR ${SUBDIRS})
    if(IS_DIRECTORY ${SRC_DIR}/${SUBDIR})
        # Add subdirectory to include path
        include_directories(${SRC_DIR}/${SUBDIR})
        
        # Find all .cpp files in this subdirectory
        file(GLOB CPP_FILES ${SRC_DIR}/${SUBDIR}/*.cpp)
        
        # Create an executable for each .cpp file
        foreach(CPP_FILE ${CPP_FILES})
            get_filename_component(EXEC_NAME ${CPP_FILE} NAME_WE)
            # Prefix with subdirectory name to avoid conflicts
            set(TARGET_NAME "josuttis11_${SUBDIR}_${EXEC_NAME}")
            
            add_executable(${TARGET_NAME} ${CPP_FILE})
            
            # Create output directory for this subdirectory
            file(MAKE_DIRECTORY "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/josuttis_cppstd11/${SUBDIR}")
            
            # Set output name to be more readable in CLion
            set_target_properties(${TARGET_NAME} PROPERTIES
                OUTPUT_NAME "${EXEC_NAME}"
                RUNTIME_OUTPUT_DIRECTORY "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/josuttis_cppstd11/${SUBDIR}"
            )
            
            # Some examples might need specific flags
            if(${SUBDIR} STREQUAL "concurrency")
                target_compile_options(${TARGET_NAME} PRIVATE -pthread)
                target_link_libraries(${TARGET_NAME} pthread)
            endif()
        endforeach()
    endif()
endforeach()

message(STATUS "Configured Josuttis C++11 examples")
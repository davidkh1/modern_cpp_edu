cmake_minimum_required(VERSION 3.20)
project(josuttis_cppstd20 CXX)

# Source directory
set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

# Add all subdirectories to include path
include_directories(${SRC_DIR})
include_directories(${SRC_DIR}/lib)

# Get all subdirectories
file(GLOB SUBDIRS RELATIVE ${SRC_DIR} ${SRC_DIR}/*)

# Process each subdirectory
foreach(SUBDIR ${SUBDIRS})
    if(IS_DIRECTORY ${SRC_DIR}/${SUBDIR})
        # Add subdirectory to include path
        include_directories(${SRC_DIR}/${SUBDIR})
        
        # Handle modules directory specially
        if(${SUBDIR} STREQUAL "modules")
            # Module support is experimental and compiler-specific
            # Check if modules are supported
            if(CMAKE_CXX_COMPILER_ID MATCHES "GNU" AND CMAKE_CXX_COMPILER_VERSION VERSION_GREATER_EQUAL "11.0")
                set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fmodules-ts")
                
                # Process module files (.cppm)
                file(GLOB MODULE_FILES ${SRC_DIR}/${SUBDIR}/*.cppm ${SRC_DIR}/${SUBDIR}/*.cppp)
                file(GLOB MODULE_CPPS ${SRC_DIR}/${SUBDIR}/*.cpp)
                
                # Create targets for module examples
                # Note: Module compilation order matters, so we'll create individual targets
                foreach(CPP_FILE ${MODULE_CPPS})
                    get_filename_component(EXEC_NAME ${CPP_FILE} NAME_WE)
                    set(TARGET_NAME "josuttis20_${SUBDIR}_${EXEC_NAME}")
                    
                    # Find associated module files
                    file(GLOB RELATED_MODULES ${SRC_DIR}/${SUBDIR}/${EXEC_NAME}*.cppm ${SRC_DIR}/${SUBDIR}/${EXEC_NAME}*.cppp)
                    
                    add_executable(${TARGET_NAME} ${CPP_FILE} ${RELATED_MODULES})
                    
                    # Create output directory
                    file(MAKE_DIRECTORY "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/josuttis_cppstd20/${SUBDIR}")
                    
                    set_target_properties(${TARGET_NAME} PROPERTIES
                        OUTPUT_NAME "${EXEC_NAME}"
                        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/josuttis_cppstd20/${SUBDIR}"
                        CXX_STANDARD 20
                        CXX_EXTENSIONS OFF
                    )
                    
                    target_compile_options(${TARGET_NAME} PRIVATE -fmodules-ts)
                endforeach()
            else()
                message(WARNING "Modules not fully supported by current compiler. Skipping modules examples.")
            endif()
        else()
            # Regular .cpp files
            file(GLOB CPP_FILES ${SRC_DIR}/${SUBDIR}/*.cpp)
            
            foreach(CPP_FILE ${CPP_FILES})
                get_filename_component(EXEC_NAME ${CPP_FILE} NAME_WE)
                set(TARGET_NAME "josuttis20_${SUBDIR}_${EXEC_NAME}")
                
                add_executable(${TARGET_NAME} ${CPP_FILE})
                
                # Create output directory
                file(MAKE_DIRECTORY "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/josuttis_cppstd20/${SUBDIR}")
                
                set_target_properties(${TARGET_NAME} PROPERTIES
                    OUTPUT_NAME "${EXEC_NAME}"
                    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/josuttis_cppstd20/${SUBDIR}"
                )
                
                # Add specific flags for certain features
                if(${SUBDIR} STREQUAL "coro")
                    # Coroutines need special flags
                    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
                        target_compile_options(${TARGET_NAME} PRIVATE -fcoroutines)
                    elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
                        target_compile_options(${TARGET_NAME} PRIVATE -fcoroutines-ts -stdlib=libc++)
                        target_link_libraries(${TARGET_NAME} c++)
                    endif()
                endif()
                
                # Some examples might need threading
                if(${SUBDIR} STREQUAL "lib" OR ${SUBDIR} STREQUAL "coro")
                    target_compile_options(${TARGET_NAME} PRIVATE -pthread)
                    target_link_libraries(${TARGET_NAME} pthread)
                endif()
            endforeach()
        endif()
    endif()
endforeach()

message(STATUS "Configured Josuttis C++20 examples")
cmake_minimum_required(VERSION 3.22)

set(PROJECT_NAME "Asteroids3D")
set(PROJECT_VERSION "1.0.0")

# Collect source files
file(GLOB_RECURSE PROJECT_SOURCES
    src/*.cpp
    src/*.hpp
    src/*.c
    src/*.h
    src/*.fs
    src/*.vs
)

# Copy assets
file(COPY ${CMAKE_SOURCE_DIR}/assets/ DESTINATION ${CMAKE_BINARY_DIR}/assets)

# GLFW VARS
set(GLFW_ROOT "${CMAKE_SOURCE_DIR}/_deps/glfw-3.4") 
set(GLFW_INCLUDE_DIR "${GLFW_ROOT}/include")
set(GLFW_LIB_DIR "${GLFW_ROOT}/lib-arm64") 

# ASSIMP VARS
set(ASSIMP_ROOT "${CMAKE_SOURCE_DIR}/_deps/assimp")
set(ASSIMP_INCLUDE_DIR "${ASSIMP_ROOT}/include")
set(ASSIMP_LIB_DIR "${ASSIMP_ROOT}/lib")

# GLM 
#set(GLM_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/include/glm")

# GLAD SOURCE
set(GLAD_SOURCE "src/glad.c")

project(
    ${PROJECT_NAME} 
    VERSION ${PROJECT_VERSION} 
    LANGUAGES C CXX
)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Include GLFW headers
include_directories(${GLFW_INCLUDE_DIR})
include_directories(${ASSIMP_INCLUDE_DIR})
# Include project headers
include_directories(include)

# Find GLFW library
find_library(GLFW_LIB_LOCATION glfw3 HINTS ${GLFW_LIB_DIR})
find_library(ASSIMP_LIB_LOCATION assimp HINTS ${ASSIMP_LIB_DIR})

add_executable(${PROJECT_NAME} ${PROJECT_SOURCES} ${GLAD_SOURCE})

# Link GLFW
target_link_libraries(${PROJECT_NAME} PRIVATE ${GLFW_LIB_LOCATION})
target_link_libraries(${PROJECT_NAME} PRIVATE ${ASSIMP_LIB_LOCATION})
# Link macOS frameworks (if building on macOS)
if(APPLE)
    target_link_libraries(${PROJECT_NAME}
        PRIVATE
            "-framework OpenGL"
            "-framework Cocoa"
            "-framework IOKit"
            "-framework CoreVideo"
            "-framework CoreFoundation"
    )
endif()

# Debugging (optional)
message("GLFW Include Dir: ${GLFW_INCLUDE_DIR}")
message("GLFW Library Dir: ${GLFW_LIB_DIR}")
message("GLFW Library Location: ${GLFW_LIB_LOCATION}")

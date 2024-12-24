cmake_minimum_required(VERSION 3.22)

set(PROJECT_NAME "Asteroids3D")
set(PROJECT_VERSION "1.0.0")

# GLFW VARS
set(GLFW_ROOT "${CMAKE_SOURCE_DIR}/_deps/glfw-3.4") 
set(GLFW_INCLUDE_DIR "${GLFW_ROOT}/include")
set(GLFW_LIB_DIR "${GLFW_ROOT}/lib-arm64") 

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

# Include project headers
include_directories(include)

# Find GLFW library
find_library(GLFW_LIB_LOCATION glfw3 HINTS ${GLFW_LIB_DIR})

add_executable(${PROJECT_NAME} src/main.cpp ${GLAD_SOURCE})

# Link GLFW
target_link_libraries(${PROJECT_NAME} PRIVATE ${GLFW_LIB_LOCATION})

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

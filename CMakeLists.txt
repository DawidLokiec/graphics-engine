cmake_minimum_required(VERSION 3.26.1)
project(graphics-engine LANGUAGES C CXX VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library(${PROJECT_NAME}
        src/library.cpp)

target_include_directories(${PROJECT_NAME} PUBLIC include/ lib/glad/include)
# A CMake pattern to have headers that are not seen by the client of this library.
# So headers which not have to be visible for client-code should be placed in src/
target_include_directories(${PROJECT_NAME} PRIVATE src ${CMAKE_CURRENT_BINARY_DIR})

if (MSVC)
    # PRIVATE specifies that the specified compile options are only for the ${PROJECT_NAME} itself,
    # and not for any targets that link to it.
    # warning level 4 and all warnings as errors
    target_compile_options(${PROJECT_NAME} PRIVATE /W4 /WX /EHsc /std:c++20)
else ()
    # PRIVATE specifies that the specified compile options are only for the ${PROJECT_NAME} itself,
    # and not for any targets that link to it.
    # lots of warnings and all warnings as errors
    target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -pedantic -Werror)
endif ()

target_link_libraries(${PROJECT_NAME} OpenGL::GL glad glfw)

### External libraries ###
# OpenGL
find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIRS})

# GLAD
add_subdirectory(lib/glad)

# GLFW
add_subdirectory(lib/glfw)
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
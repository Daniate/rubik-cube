cmake_minimum_required(VERSION 3.0.0)
project(rubik-cube VERSION 0.1.0)

# check c++11 support
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)

if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

if (POLICY CMP0072)
  set(OpenGL_GL_PREFERENCE LEGACY)
endif()

set(CMAKE_SHARED_LIBRARY_RUNTIME_C_FLAG "-Wl,-rpath,")

find_package(GLEW REQUIRED CONFIG)
find_package(glfw3 REQUIRED)
find_package(OpenGL REQUIRED)

set(VAR_EXE solver)

aux_source_directory(src VAR_SRC)

add_executable(
    ${VAR_EXE} 
    example/solver.cpp
    ${VAR_SRC}
)

target_include_directories(${VAR_EXE} PRIVATE ${OPENGL_INCLUDE_DIR} include)
target_link_libraries(${VAR_EXE} PRIVATE GLEW::GLEW glfw ${OPENGL_LIBRARIES} m)
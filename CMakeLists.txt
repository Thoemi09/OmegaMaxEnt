#CMakeLists.txt modified by Moise Rousseau (September 21st 2022)

cmake_minimum_required (VERSION 3.18)

project(
    OmegaMaxEnt 
    LANGUAGES CXX 
    VERSION 1.0 
    DESCRIPTION "Analytic continuation of imaginary time/frequency Green or response functions"
)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_executable(OmegaMaxEnt 
    cpp/graph_2D.cpp
    cpp/graph_3D.cpp
    cpp/generique.cpp
    cpp/OmegaMaxEnt_data.cpp
    cpp/OmegaMaxEnt_main.cpp
)

# Find/fetch dependencies (Armadillo, LAPACK, FFTW3)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/external)
find_package(LAPACK REQUIRED)
find_package(FFTW REQUIRED)

set(ARMA_DONT_USE_WRAPPER ON CACHE BOOL "Don't use wrapper library" FORCE)
set(HEADER_ONLY ON CACHE BOOL "Use Armadillo header-only mode")
include(external/CPM.cmake)
CPMAddPackage(
    NAME armadillo
    GIT_REPOSITORY https://gitlab.com/conradsnicta/armadillo-code.git
    GIT_TAG 15.0.1
    GIT_SHALLOW TRUE
)

# Link executable to dependencies
target_include_directories(OmegaMaxEnt PRIVATE cpp ${LAPACK_INCLUDE_DIRS})
target_link_libraries(OmegaMaxEnt PRIVATE ${LAPACK_LIBRARIES} FFTW::Double armadillo)

# Install executable
install(TARGETS OmegaMaxEnt DESTINATION bin)

cmake_minimum_required(VERSION 3.18) # Required for CUDA language support
project(particle_branch_11 LANGUAGES CXX CUDA)

# Specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(PROJECT_SOURCE_DIR "src")
#set(CMAKE_CUDA_COMPILER "/usr/local/cuda-12.3/bin/nvcc") # Change for your system: $ whereis nvcc
# Set CUDA flags
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -lineinfo")

include_directories(# Add CUDA toolkit include directories
                    ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})

# Define source and header files for the project
set(SOURCE_FILES ${PROJECT_SOURCE_DIR}/QThrusterMain.cu
                 # Add any .cpp files if necessary
)

set(HEADER_FILES
    ${PROJECT_SOURCE_DIR}/include/AddParticlesGPU.cuh
    ${PROJECT_SOURCE_DIR}/include/BoxConeCollider.cuh
    ${PROJECT_SOURCE_DIR}/include/CalculateCellVolumesGPU.cuh
    ${PROJECT_SOURCE_DIR}/include/DataIO.cuh
    ${PROJECT_SOURCE_DIR}/include/FillGapsCPU.cuh
    ${PROJECT_SOURCE_DIR}/include/FillGapsGPU.cuh
    ${PROJECT_SOURCE_DIR}/include/gpuErrchk.cuh
    ${PROJECT_SOURCE_DIR}/include/Load_EB.cuh
    ${PROJECT_SOURCE_DIR}/include/OtherFunctions.cuh
    ${PROJECT_SOURCE_DIR}/include/ParticleMoverCPU.cuh
    ${PROJECT_SOURCE_DIR}/include/ParticleMoverGPU.cuh
    ${PROJECT_SOURCE_DIR}/include/ThrustCalculationGPU.cuh
    ${PROJECT_SOURCE_DIR}/include/ThrustCalculationGPU1.cuh
    ${PROJECT_SOURCE_DIR}/include/ThrustCalculationGPU2.cuh
    ${PROJECT_SOURCE_DIR}/include/TimerWrappers.cuh
    ${PROJECT_SOURCE_DIR}/include/TransferParticlesCPU.cuh
    ${PROJECT_SOURCE_DIR}/include/TransferParticlesGPU.cuh
    ${PROJECT_SOURCE_DIR}/include/WriteForce.cuh
    ${PROJECT_SOURCE_DIR}/include/WriteToSnowballGPU.cuh
    ${PROJECT_SOURCE_DIR}/include/WriteVTK.cuh
)

# Add executable target with source and header files
add_executable(particle_branch_11 ${SOURCE_FILES} ${HEADER_FILES})

# Specify include directories
include_directories(
  "${PROJECT_SOURCE_DIR}/include"
)

# Find CUDA
#find_package(CUDA 10.0 REQUIRED) # Adjust based on your CUDA version

# Link CUDA libraries
target_link_libraries(particle_branch_11 ${CUDA_LIBRARIES}
                      ${CUDA_CUDART_LIBRARY})

# Specify additional options for the CUDA compiler
set_target_properties(particle_branch_11 PROPERTIES 
                      CUDA_SEPARABLE_COMPILATION ON
                     )

# Set CUDA specific options
#target_compile_options(particle_branch_11 PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:-lineinfo>)

# Copy the configuration text file to the build directory
configure_file(${PROJECT_SOURCE_DIR}/input_deck_2_7.txt
               ${CMAKE_CURRENT_BINARY_DIR}/input_deck_2_7.txt COPYONLY)
# Copy the COMSOL field files to the build directory
file(COPY ${PROJECT_SOURCE_DIR}/complex DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

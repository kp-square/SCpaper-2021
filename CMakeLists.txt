cmake_minimum_required(VERSION 3.1)
project(ET CUDA CXX)
FIND_PACKAGE(OpenMP REQUIRED)
if (NOT DEFINED CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 17)
    set(CMAKE_CXX_STANDARD_REQUIRED True)
endif()

if (NOT DEFINED CMAKE_CUDA_STANDARD)
    set(CMAKE_CUDA_STANDARD 17)
    set(CMAKE_CUDA_STANDARD_REQUIRED True)
endif()
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} --expt-relaxed-constexpr --extended-lambda --default-stream per-thread -Xcompiler=${OpenMP_CXX_FLAGS}")
SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_CXX_FLAGS}")  
set(CMAKE_CUDA_ARCHITECTURES 70)
# added because cnpy was installed locally
set(CNPY_INSTALL_DIR "$ENV{HOME}/local/cnpy")

include_directories(
    ${CUDA_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/
    ${CMAKE_CURRENT_SOURCE_DIR}/mycublas/
    ${CMAKE_CURRENT_SOURCE_DIR}/mat_type/
    # added because cnpy was installed locally
    ${CNPY_INSTALL_DIR}/include/
    )

# added because cnpy was installed locally
link_directories(${CNPY_INSTALL_DIR}/lib)

add_subdirectory(mycublas)
add_subdirectory(lib)
add_subdirectory(mat_type)
add_subdirectory(Linear)
add_subdirectory(Attention)
add_subdirectory(LayerNorm)
add_subdirectory(SkipLayerNorm)
add_subdirectory(Encoder)
add_subdirectory(Fused_attention)
add_subdirectory(test)
add_subdirectory(example)



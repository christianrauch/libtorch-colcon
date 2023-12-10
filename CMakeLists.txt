cmake_minimum_required(VERSION 3.8)
project(libtorch)

include(ExternalProject)

ExternalProject_Add(${PROJECT_NAME}
  GIT_REPOSITORY          https://github.com/pytorch/pytorch.git
  GIT_TAG                 v2.1.1
  GIT_SUBMODULES_RECURSE  ON
  GIT_SHALLOW             ON
  GIT_PROGRESS            ON

  CMAKE_GENERATOR         Ninja
  CMAKE_ARGS              -D BUILD_SHARED_LIBS:BOOL=ON
                          -D CMAKE_BUILD_TYPE:STRING=Release
                          -D CMAKE_CXX_STANDARD=17
                          -D BUILD_PYTHON:BOOL=OFF
                          -D USE_CUDA:BOOL=ON
                          -D USE_NCCL:BOOL=OFF
                          -D USE_MAGMA:BOOL=ON
                          -D USE_TENSORRT:BOOL=OFF
                          -D USE_VULKAN:BOOL=OFF
                          -D USE_MKLDNN:BOOL=OFF
)

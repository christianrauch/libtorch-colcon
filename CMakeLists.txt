cmake_minimum_required(VERSION 3.8)
project(libtorch)

include(ExternalProject)

ExternalProject_Add(${PROJECT_NAME}
  URL                     https://github.com/pytorch/pytorch/releases/download/v2.1.2/pytorch-v2.1.2.tar.gz
  URL_HASH                MD5=99afc663efe0dfe7fb8296b222e6eec8

  CMAKE_GENERATOR         Ninja
  CMAKE_ARGS              -D BUILD_SHARED_LIBS:BOOL=ON
                          -D CMAKE_BUILD_TYPE:STRING=Release
                          # -D CMAKE_CXX_STANDARD=17
                          # -D BUILD_PYTHON:BOOL=OFF
                          # -D USE_CUDA:BOOL=ON
                          # -D USE_NCCL:BOOL=OFF
                          # -D USE_MAGMA:BOOL=ON
                          # -D USE_TENSORRT:BOOL=OFF
                          # -D USE_VULKAN:BOOL=OFF
                          # -D USE_MKLDNN:BOOL=OFF
)

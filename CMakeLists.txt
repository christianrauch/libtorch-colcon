cmake_minimum_required(VERSION 3.22)
project(libtorch)

include(ExternalProject)

ExternalProject_Add(${PROJECT_NAME}
  URL                     https://github.com/pytorch/pytorch/releases/download/v2.1.2/pytorch-v2.1.2.tar.gz
  URL_HASH                MD5=99afc663efe0dfe7fb8296b222e6eec8

  # CMAKE_GENERATOR         Ninja

  # CMAKE_ARGS
  #   -D BUILD_SHARED_LIBS:BOOL=ON
  #   -D CMAKE_BUILD_TYPE:STRING=Release
  #   # -D CMAKE_CXX_STANDARD=17
  #   -D BUILD_PYTHON:BOOL=OFF
  #   -D USE_CUDA:BOOL=ON
  #   -D USE_CUDNN:BOOL=ON
  #   -D USE_CUSPARSELT:BOOL=OFF
  #   -D USE_NCCL:BOOL=ON
  #   -D USE_MAGMA:BOOL=ON
  #   # -D USE_TENSORRT:BOOL=OFF
  #   # -D USE_VULKAN:BOOL=OFF
  #   # -D USE_MKLDNN:BOOL=OFF
  #   -D CMAKE_SHARED_LINKER_FLAGS="-fno-lto"

  CONFIGURE_COMMAND
    COMMAND ${CMAKE_COMMAND}
      -S ${CMAKE_BINARY_DIR}/${PROJECT_NAME}-prefix/src/${PROJECT_NAME}
      -B pytorch-build
      -G Ninja
      -D BUILD_SHARED_LIBS:BOOL=ON
      -D CMAKE_BUILD_TYPE:STRING=Release
      -D BUILD_PYTHON:BOOL=OFF
      -D USE_CUDA:BOOL=ON
      -D USE_CUDNN:BOOL=ON
      -D USE_CUSPARSELT:BOOL=OFF
      -D USE_NCCL:BOOL=ON
      -D USE_MAGMA:BOOL=ON
      -D CMAKE_SHARED_LINKER_FLAGS="-fno-lto"
      -D CMAKE_INSTALL_PREFIX:PATH=../pytorch-install
      # -D CMAKE_INSTALL_PREFIX:PATH=${CMAKE_INSTALL_PREFIX}

  BUILD_COMMAND
    COMMAND ${CMAKE_COMMAND} --build pytorch-build --target install

  INSTALL_COMMAND ""
  # INSTALL_COMMAND
  #   COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_BINARY_DIR}/${PROJECT_NAME}-prefix/src/${PROJECT_NAME}-build/pytorch-install ${CMAKE_INSTALL_PREFIX}
)

# ExternalProject_Add(${PROJECT_NAME}
#   URL                     https://github.com/pytorch/pytorch/releases/download/v2.1.2/pytorch-v2.1.2.tar.gz
#   URL_HASH                MD5=99afc663efe0dfe7fb8296b222e6eec8

#   CONFIGURE_COMMAND       ""
#   BUILD_COMMAND           ""
#     BUILD_TEST=OFF
#     USE_CUDA=ON
#     USE_CUDNN=ON
#     CMAKE_SHARED_LINKER_FLAGS="-fno-lto"
#       python3 ../${PROJECT_NAME}/tools/build_libtorch.py
#   INSTALL_COMMAND         ""
# )


ExternalProject_Get_Property(${PROJECT_NAME} BINARY_DIR)
set(TORCH_PATH ${BINARY_DIR}/pytorch-install)

include(GNUInstallDirs)

install(DIRECTORY ${TORCH_PATH}/include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILES_MATCHING PATTERN "*.h*"
)

install(DIRECTORY ${TORCH_PATH}/lib/
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
    FILES_MATCHING PATTERN "*.so*"
)

install(DIRECTORY ${TORCH_PATH}/share/
    DESTINATION ${CMAKE_INSTALL_DATADIR}
)

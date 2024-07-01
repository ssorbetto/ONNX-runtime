cmake_minimum_required(VERSION 3.20)

project(ONNX_Runtime_Eye)
set(CMAKE_CXX_STANDARD 17) #14
set(CMAKE_BUILD_TYPE RelWithDebInfo)
set(CMAKE_CUDA_ARCHITECTURES native)
set(CMAKE_CUDA_COMPILER /usr/local/cuda-12.2/bin/nvcc)

# # attempt to add PyTorch cpp wrapper
# set(CMAKE_PREFIX_PATH "/scratch/lucy/libtorch")
# find_package(Torch REQUIRED)
# # set(TORCH_LIBRARIES ${TORCH_LIBRARIES})
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

# set(TORCH_LIBRARIES /scratch/lucy/libtorch)
add_executable(ONNX_Runtime_Eye src/main.cpp src/image_classifier.cpp)
# include_directories(${TORCH_INCLUDE_DIRS})
target_link_libraries(ONNX_Runtime_Eye ${TORCH_LIBRARIES})
#set_property(TARGET ONNX_Runtime_Eye PROPERTY CXX_STANDARD 14) #17
#set_property(TARGET ONNX_Runtime_Eye PROPERTY CUDA_ARCHITECTURES native)

# TODO: Change the path to your ONNX Runtime installation directory
set(ONNXRUNTIME_ROOT_PATH /scratch/lucy)
set(ONNXRUNTIME_INCLUDE_DIRS
  ${ONNXRUNTIME_ROOT_PATH}/onnxruntime/include/onnxruntime
  ${ONNXRUNTIME_ROOT_PATH}/onnxruntime
  ${ONNXRUNTIME_ROOT_PATH}/onnxruntime/include/onnxruntime/core/session
)

set(ONNXRUNTIME_LIB ${ONNXRUNTIME_ROOT_PATH}/onnxruntime/build/Linux/Release/libonnxruntime.so)
include_directories(${ONNXRUNTIME_INCLUDE_DIRS})
target_link_libraries(ONNX_Runtime_Eye ${ONNXRUNTIME_LIB})

set(OpenCV_DIR /scratch/lucy/opencv-4.x)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
target_link_libraries(ONNX_Runtime_Eye ${OpenCV_LIBS})

add_subdirectory(src)
# message(STATUS "PyTorch Include Dirs: ${TORCH_INCLUDE_DIRS}")
# message(STATUS "PyTorch Libraries: ${TORCH_LIBRARIES}")
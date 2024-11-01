cmake_minimum_required(VERSION 3.10)

project(yolov8)

# Modify to your path
set(OpenCV_DIR "E:/opencv/build")  
set(TRT_DIR "E:/TensorRT-8.6.1.6")  
set(TRT_INCLUDE_DIR "E:/TensorRT-8.6.1.6/include")
set(TRT_LIB_DIR "E:/TensorRT-8.6.1.6/lib")

add_definitions(-std=c++11)
add_definitions(-DAPI_EXPORTS)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Debug)

# setup CUDA
find_package(CUDAToolkit REQUIRED)
message(STATUS "libraries: ${CUDA_LIBRARIES}")
message(STATUS "include path: ${CUDA_INCLUDE_DIRS}")
include_directories(${CUDA_INCLUDE_DIRS})

enable_language(CUDA)

include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(${PROJECT_SOURCE_DIR}/plugin)

# TensorRT
#find_package(TRT_DIR)
#include_directories(${TRT_DIR}/include)
#link_directories(${TRT_DIR}/lib)

# 添加 TensorRT 头文件目录到 include_directories
include_directories(${TRT_INCLUDE_DIR})

# 添加 TensorRT 库文件目录到 link_directories
link_directories(${TRT_LIB_DIR})

# 查找 TensorRT 相关的库文件
find_library(NVINFER_LIB nvinfer HINTS ${TRT_LIB_DIR})
find_library(NVPARSERS_LIB nvparsers HINTS ${TRT_LIB_DIR})
find_library(NVONNXPARSER_LIB nvonnxparser HINTS ${TRT_LIB_DIR})
find_library(CUDART_LIB cudart HINTS ${TRT_LIB_DIR})
find_library(CUBLAS_LIB cublas HINTS ${TRT_LIB_DIR})

# OpenCV
find_package(OpenCV)
include_directories(${OpenCV_INCLUDE_DIRS})

add_library(myplugins SHARED ${PROJECT_SOURCE_DIR}/plugin/yololayer.cu)
target_link_libraries(myplugins nvinfer cudart)

file(GLOB_RECURSE SRCS ${PROJECT_SOURCE_DIR}/src/*.cpp ${PROJECT_SOURCE_DIR}/src/*.cu)
add_executable(yolov8 ${PROJECT_SOURCE_DIR}/src/main.cpp ${SRCS})

target_link_libraries(yolov8 nvinfer)
target_link_libraries(yolov8 cudart)
target_link_libraries(yolov8 myplugins)
target_link_libraries(yolov8 ${OpenCV_LIBS})


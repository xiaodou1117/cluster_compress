cmake_minimum_required(VERSION 3.10)
project(delta_compression_framework)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_C_STANDARD 99)
find_package(Glog REQUIRED)
set(CMAKE_BUILD_TYPE Release)

include_directories(${CMAKE_SOURCE_DIR})
add_subdirectory(chunk)
add_subdirectory(feature)
add_subdirectory(utils)
add_subdirectory(index)
add_subdirectory(storage)
add_subdirectory(3party/xdelta3)
add_subdirectory(encoder)
# add_subdirectory(experiments)
add_library(cluster_compress STATIC delta_compression.cpp pipeline_delta_compression.cpp)
target_link_libraries(cluster_compress chunker feature index storage utils xdelta3 glog gflags lz4 gsl gslcblas zstd)
add_executable(cluster_compression main.cpp)
target_link_libraries(cluster_compression cluster_compress stdc++fs libgflags.a libglog.so libgtest.so liblz4.a libgsl.a libgslcblas.a libzstd.a)

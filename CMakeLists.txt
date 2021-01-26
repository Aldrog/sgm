#    This file is part of sgm. (https://github.com/dhernandez0/sgm).
#
#    Copyright (c) 2016 Daniel Hernandez Juarez.

#    sgm is free software: you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.

#    sgm is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.

#    You should have received a copy of the GNU General Public License
#    along with sgm.  If not, see <http://www.gnu.org/licenses/>.

cmake_minimum_required(VERSION 3.17)
project(sgm LANGUAGES CUDA)
set(CMAKE_CUDA_STANDARD 14)

find_package(OpenCV REQUIRED)
find_package(CUDAToolkit REQUIRED)

if(NOT CMAKE_CUDA_ARCHITECTURES)
    set(CMAKE_CUDA_ARCHITECTURES "75")
endif()

add_library(
    sgm_lib STATIC
    median_filter.cu hamming_cost.cu disparity_method.cu debug.cu costs.cu)
target_link_libraries(sgm_lib PUBLIC ${OpenCV_LIBS} CUDA::cudart)
target_include_directories(sgm_lib PUBLIC ${CMAKE_CURRENT_LIST_DIR})

add_executable(sgm EXCLUDE_FROM_ALL main.cu)
target_link_libraries(sgm sgm_lib ${OpenCV_LIBS})

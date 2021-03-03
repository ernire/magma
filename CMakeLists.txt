cmake_minimum_required(VERSION 3.12)
project(magma-bms)

set(OMP ON)
set(CUDA OFF)

set(CMAKE_CXX_STANDARD 17)
#add_executable(DEEP svm_light_to_bin.cpp)
#add_executable(next-svm-light-to-bin source/svm_light_to_bin.cpp source/next_svm_io.cpp source/next_svm_io.h
#        source/test_util.cpp source/test_util.h)
#add_executable(next-svm-predict source/next_svm_predict.cpp source/next_svm_predict.h source/next_svm_io.cpp
#        source/next_svm_io.h source/test_util.cpp source/test_util.h source/pisvm.cpp source/pisvm.h)

if (CUDA)
    add_definitions(-DCUDA_ON)
    set(CMAKE_CUDA_STANDARD 20)
    add_executable(magma-cu-bms exa_bms.cu)
    set_target_properties(magma-cu-bms PROPERTIES
            COMPILE_FLAGS "--expt-extended-lambda")
else()
    set(CMAKE_CXX_STANDARD 20)
    set(CMAKE_CXX_FLAGS "-Wall -Wextra -O3 -fno-exceptions")
    add_executable(magma-bms exa_bms.cpp)
    if (OMP)
        add_definitions(-DOMP_ON)
        find_package(OpenMP REQUIRED)
        target_link_libraries(magma-bms PUBLIC OpenMP::OpenMP_CXX)
    endif()
endif()
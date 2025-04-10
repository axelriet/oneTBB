# Copyright (c) 2020-2025 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required(VERSION 3.5.0...3.31.3)

project(parallel_preorder CXX)

include(../../common/cmake/common.cmake)

set_common_project_settings(tbb)

add_executable(parallel_preorder parallel_preorder.cpp Graph.cpp main.cpp)

target_link_libraries(parallel_preorder TBB::tbb Threads::Threads)
target_compile_options(parallel_preorder PRIVATE ${TBB_CXX_STD_FLAG})

set(EXECUTABLE "$<TARGET_FILE:parallel_preorder>")
set(ARGS "")
set(PERF_ARGS auto silent 500000 100)
set(LIGHT_ARGS 1:auto:+4 n-of-traversals=50)

add_execution_target(run_parallel_preorder parallel_preorder ${EXECUTABLE} "${ARGS}")
add_execution_target(perf_run_parallel_preorder parallel_preorder ${EXECUTABLE} "${PERF_ARGS}")
add_execution_target(light_test_parallel_preorder parallel_preorder ${EXECUTABLE} "${LIGHT_ARGS}")
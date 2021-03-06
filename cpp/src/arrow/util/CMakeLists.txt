# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

#######################################
# arrow_util
#######################################

file(GLOB_RECURSE ARROW_UTIL_HEADERS "*.h")

# Headers: top level
install(FILES
  ${ARROW_UTIL_HEADERS}
  DESTINATION include/arrow/util)

#######################################
# arrow_test_main
#######################################

if (ARROW_BUILD_BENCHMARKS)
  add_library(arrow_benchmark_main benchmark_main.cc)
  if (APPLE)
    target_link_libraries(arrow_benchmark_main
      benchmark
    )
  elseif(MSVC)
    target_link_libraries(arrow_benchmark_main
      benchmark
      Shlwapi.lib
  )
  else()
    target_link_libraries(arrow_benchmark_main
      benchmark
      pthread
    )
  endif()

  # TODO(wesm): Some benchmarks include gtest.h
  add_dependencies(arrow_benchmark_main gtest)
endif()

ADD_ARROW_TEST(bit-util-test)
ADD_ARROW_TEST(checked-cast-test)
ADD_ARROW_TEST(compression-test)
ADD_ARROW_TEST(decimal-test)
ADD_ARROW_TEST(key-value-metadata-test)
ADD_ARROW_TEST(rle-encoding-test)
ADD_ARROW_TEST(stl-util-test)

ADD_ARROW_BENCHMARK(bit-util-benchmark)
ADD_ARROW_BENCHMARK(decimal-benchmark)

add_subdirectory(variant)

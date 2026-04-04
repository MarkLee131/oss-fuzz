# Copyright 2016 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder
RUN apt-get update -y && apt-get install -y cmake make pkg-config

# https://github.com/abseil/abseil-cpp/issues/1524#issuecomment-1739364093
# explains why Abseil must be built for fuzzing, not depended on normally.
RUN git clone --depth=1 https://github.com/abseil/abseil-cpp
RUN git clone --depth 1 https://github.com/google/re2

# Pre-install abseil headers for LogicFuzz wllvm compilation
# (cmake doesn't work well with wllvm wrapper, so we install headers first)
RUN cd $SRC/abseil-cpp && mkdir -p build && cd build && \
    cmake -DCMAKE_POSITION_INDEPENDENT_CODE=ON .. && \
    make -j$(nproc) && make install && ldconfig

COPY build.sh $SRC/

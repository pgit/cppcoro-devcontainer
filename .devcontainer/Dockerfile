# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.134.0/containers/cpp/.devcontainer/base.Dockerfile
ARG VARIANT="buster"
FROM mcr.microsoft.com/vscode/devcontainers/cpp:0-${VARIANT}

#
# Debian: CMake, git
#
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get -y upgrade \
    && apt-get -y install cmake git libc++abi-dev libc++-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

#
# Latest LLVM (beyond version 7 already installed using apt-get)
#
# https://pspdfkit.com/blog/2020/visual-studio-code-cpp-docker/
#
ARG LLVM_VERSION=12
ARG LLVM_GPG_FINGERPRINT=6084F3CF814B57C1CF12EFD515CF4D18AF4F7421

RUN wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add - \
    && echo "deb http://apt.llvm.org/buster/ llvm-toolchain-buster-${LLVM_VERSION} main" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get -y install --no-install-recommends \
        llvm-${LLVM_VERSION} \
        clang-${LLVM_VERSION} \
        lldb-${LLVM_VERSION} \
        libc++-${LLVM_VERSION}-dev \
        libc++abi-${LLVM_VERSION}-dev \
        clang-tidy-${LLVM_VERSION} \
        clangd-${LLVM_VERSION} \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV DEBIAN_FRONTEND=

#
# Boost
#
ARG BOOST_VERSION=1_75_0
RUN wget https://dl.bintray.com/boostorg/release/1.75.0/source/boost_${BOOST_VERSION}.tar.bz2 \
    && tar xjvf boost_${BOOST_VERSION}.tar.bz2 \
    && rm boost_${BOOST_VERSION}.tar.bz2 \
    && cd boost_${BOOST_VERSION} \
    && ./bootstrap.sh --with-toolset=clang \
    && ./b2 -j8 --with-system --with-thread --with-date_time --with-regex --with-serialization --with-filesystem install \
    && cd .. \
    && rm -rf boost_${BOOST_VERSION}

# RUN apt-get -y remove clang-7 libclang-common-7-dev libclang1-7 libomp-7-dev libomp5-7

# ENV CC=/usr/bin/clang-${LLVM_VERSION}
# ENV CXX=/usr/bin/clang++-${LLVM_VERSION}

# RUN update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-${LLVM_VERSION} 100 \
#     && update-alternatives --install /usr/bin/clang clang /usr/bin/clang-${LLVM_VERSION} 100

#SHELL ["/bin/bash", "-c"]
#RUN cd /opt \
#    && git clone --recursive https://github.com/lewissbaker/cppcoro.git cppcoro \
#    && cd cppcoro \
#    && source init.sh \
#    && cake --clang-executable=clang-${LLVM_VERSION} release=debug

ENV CC="/usr/bin/clang-${LLVM_VERSION}" \
    CXX="/usr/bin/clang++-${LLVM_VERSION}" \
    CXXFLAGS="-stdlib=libc++" \
    COV="/usr/bin/llvm-cov-${LLVM_VERSION}" \
    LLDB="/usr/bin/lldb-${LLVM_VERSION}"

RUN git clone https://github.com/andreasbuhr/cppcoro \
    && cd cppcoro \
    && mkdir build \
    && cd build \
    && cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr .. \
    && make -j8 install \
    && cd ../.. \
    && rm -rf cppcoro

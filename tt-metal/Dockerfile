FROM mcr.microsoft.com/devcontainers/base:ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# Update the container
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
    build-essential \
    git \
    curl \
    wget 

RUN apt-get install software-properties-common -y && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y \
    python3.12-full \
    python3.12-dev \
    libpython3.12-dev \
    python3.12-venv

# Install clang-17
RUN wget https://apt.llvm.org/llvm.sh && \
    chmod +x llvm.sh && \
    ./llvm.sh 17 && \
    rm llvm.sh

# Nice-to-haves
RUN apt-get install -y \
    bash-completion \
    git \
    build-essential \
    ninja-build \
    pkg-config \
    pandoc \
    xz-utils \
    libhwloc-dev \
    libnuma-dev \
    libatomic1 \
    libstdc++6 \
    libboost-dev \
    libtbb-dev \
    libcapstone-dev \
    libc++-17-dev \
    libc++abi-17-dev \
    wget \
    ccache \
    sudo \
    xxd

# Install Python3.12 as symlink to python[3]
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.12 1
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.12 1

# Install cmake 3.24
RUN python3 -m ensurepip && \
    python3 -m pip install --upgrade pip && \
    python3 -m pip install cmake==3.24.2

# Install openmpi-5.0.7
RUN wget https://download.open-mpi.org/release/open-mpi/v5.0/openmpi-5.0.7.tar.gz && \
    tar -xvf openmpi-5.0.7.tar.gz && \
    cd openmpi-5.0.7 && \
    ./configure --prefix=/opt/openmpi-5.0.7 && \
    make -j$(nproc) all && \
    make install && \
    ldconfig && \
    cd .. && \
    rm -rf openmpi-5.0.7 openmpi-5.0.7.tar.gz

RUN ln /usr/bin/clangd-17 /usr/bin/clangd

# Install Rust for user
USER vscode
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/home/vscode/.cargo/bin:${PATH}"

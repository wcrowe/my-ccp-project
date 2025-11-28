FROM mcr.microsoft.com/devcontainers/cpp:1-ubuntu-24.04

# Install prerequisites
RUN apt-get update && apt-get install -y \
    wget \
    gnupg \
    software-properties-common \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Add LLVM GPG key
RUN wget -qO- https://apt.llvm.org/llvm-snapshot.gpg.key | tee /etc/apt/trusted.gpg.d/apt.llvm.org.asc

# Add LLVM 21 repo for Ubuntu 24.04 (noble)
RUN echo "deb http://apt.llvm.org/noble/ llvm-toolchain-noble-21 main" | tee /etc/apt/sources.list.d/llvm-21.list

# Install Clang 21 and related tools
RUN apt-get update && apt-get install -y \
    clang-21 \
    clang-tools-21 \
    clang-format-21 \
    clang-tidy-21 \
    libclang-21-dev \
    lld-21 \
    lldb-21 \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Set Clang 21 as default using update-alternatives (priority 2100 to override older versions)
RUN update-alternatives --install /usr/bin/clang clang /usr/bin/clang-21 2100 \
    && update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-21 2100 \
    && update-alternatives --install /usr/bin/clang-format clang-format /usr/bin/clang-format-21 2100 \
    && update-alternatives --install /usr/bin/clang-tidy clang-tidy /usr/bin/clang-tidy-21 2100 \
    && update-alternatives --install /usr/bin/run-clang-tidy run-clang-tidy /usr/bin/run-clang-tidy-21 2100 \
    && update-alternatives --install /usr/bin/lld lld /usr/bin/lld-21 2100 \
    && update-alternatives --install /usr/bin/lldb lldb /usr/bin/lldb-21 2100
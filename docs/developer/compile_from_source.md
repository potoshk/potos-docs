# Build FISCO BCOS from source

FISCO BCOS uses the `CMake` build system to generate platform-specific build files, which means the
workflow is very similar no matter what operating system you use:

- Install build tools and dependencies (platform dependent)
- Clone the repository
- Run cmake to generate build files and compile

## Prerequisites

### For Apt-based Linux distributions

It is recommended to use Ubuntu 22.04 version or later.

```bash
apt update
apt install -y wget python3-dev git curl zip unzip tar
apt install -y --no-install-recommends clang make build-essential cmake libssl-dev zlib1g-dev ca-certificates libgmp-dev flex bison patch libzstd-dev ninja-build pkg-config autoconf


# Install Rust
curl https://sh.rustup.rs -sSf | bash -s -- -y
source $HOME/.cargo/env
```

### For Yum-based Linux distributions

It is recommended to use CentOS7 or later.

```bash
yum install -y epel-release centos-release-scl flex bison patch gmp-static
yum install -y devtoolset-10 devtoolset-11 llvm-toolset-7 rh-perl530-perl cmake3 zlib-devel ccache lcov python-devel python3-devel

# upgrade git to version 2.17
yum reinstall -y https://packages.endpointdev.com/rhel/7/os/x86_64/endpoint-repo.x86_64.rpm
yum install -y git
```

### For macOS

It is recommended to use macOS 15.0 or later.

```bash
brew install cmake ninja pkg-config openssl@1.1 git zstd wget gmp
# Install Rust
curl https://sh.rustup.rs -sSf | bash -s -- -y
source $HOME/.cargo/env
```

## Compile

Compile FISCO BCOS with the following steps:

```bash
# Into the Universal-BCOS directory
cd Universal-BCOS
# Create a build directory
mkdir build
# Enter the build directory
cd build
# Generate build files
cmake .. -DBUILD_STATIC=ON
# Compile, the -j4 option specifies the number of threads to use
make -j4
```

## Run

After compiling, you can use `build_chain.sh` script to specify executable binary files for building a local chain.

```bash
# Into the tool directory
cd tools/BcosAirBuilder
# Build a local chain, and it will generate a directory named nodes/
bash build_chain.sh -l "127.0.0.1:4" -p 20200,30300 -e ../../build/universal-bcos-air/universal-bcos
# Start the local chain
cd nodes/127.0.0.1/
bash start_all.sh
```

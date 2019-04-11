Kendryte RISC-V GNU Compiler Toolchain
=============================

This is the Kendryte RISC-V C and C++ cross-compiler. It supports two build modes:
a generic ELF/Newlib toolchain and a more sophisticated Linux-ELF/glibc
toolchain.

###  Getting the sources

This repository uses submodules. You need the --recursive option to fetch the submodules automatically

    $ git clone --recursive https://github.com/kendryte/kendryte-gnu-toolchain
    
Alternatively :

    $ git clone https://github.com/kendryte/kendryte-gnu-toolchain
    $ cd kendryte-gnu-toolchain
    $ git submodule update --init --recursive
    
    

### Prerequisites

Several standard packages are needed to build the toolchain.  On Ubuntu,
executing the following command should suffice:

    $ sudo apt-get install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev

On Fedora/CentOS/RHEL OS, executing the following command should suffice:

    $ sudo yum install autoconf automake libmpc-devel mpfr-devel gmp-devel gawk  bison flex texinfo patchutils gcc gcc-c++ zlib-devel expat-devel

On OS X, you can use [Homebrew](http://brew.sh) to install the dependencies:

    $ brew install gawk gnu-sed gmp mpfr libmpc isl zlib expat

To build the glibc (Linux) on OS X, you will need to build within a case-sensitive file
system.  The simplest approach is to create and mount a new disk image with
a case sensitive format.  Make sure that the mount point does not contain spaces. This is not necessary to build newlib or gcc itself on OS X.

This process will start by downloading about 200 MiB of upstream sources, then
will patch, build, and install the toolchain.  If a local cache of the
upstream sources exists in $(DISTDIR), it will be used; the default location
is /var/cache/distfiles.  Your computer will need about 8 GiB of disk space to
complete the process.

### Installation (Newlib)

To build the Newlib cross-compiler, pick an install path.  If you choose,
say, `/opt/kendryte-toolchain`, then add `/opt/kendryte-toolchain/bin` to your `PATH` now.  Then, simply
run the following command:


```bash
./configure --prefix=/opt/kendryte-toolchain --with-cmodel=medany --with-arch=rv64imafc --with-abi=lp64f
make -j8
```

You should now be able to use riscv64-unknown-elf-gcc and its cousins.

- Linux

Ensure you have write access to ```/opt/kendryte-toolchain```

```bash
./configure --prefix=/opt/kendryte-toolchain --with-cmodel=medany --with-arch=rv64imafc --with-abi=lp64f

make -j8
```

Static link some libraries:

```bash
git clone --recursive https://github.com/kendryte/kendryte-gnu-toolchain
cd kendryte-gnu-toolchain
cd riscv-gcc
./contrib/download_prerequisites
cd ..
./configure --prefix=/opt/kendryte-toolchain --with-cmodel=medany --with-arch=rv64imafc --with-abi=lp64f
make -j8
```

- OSX

```bash
./configure --prefix=/usr/local/opt/kendryte-toolchain --with-cmodel=medany --with-arch=rv64imafc --with-abi=lp64f
make -j8
```

- Windows

Must cross-compile under linux using mingw.

Install mingw.

```
sudo apt install binutils-mingw-w64 binutils-mingw-w64-i686 binutils-mingw-w64-x86-64 g++-mingw-w64 g++-mingw-w64-i686 g++-mingw-w64-x86-64 gcc-mingw-w64 gcc-mingw-w64-base gcc-mingw-w64-i686 gcc-mingw-w64-x86-64 libz-mingw-w64-dev mingw-w64-common mingw-w64-i686-dev mingw-w64-x86-64-dev

```

Hack the zlib.
```bash
xz -9 /usr/i686-w64-mingw32/lib/libz.dll.a
xz -9 /usr/i686-w64-mingw32/lib/zlib1.dll
```

Compile expat-2.2.6
```bash
apt install docbook2x

export CC=i686-w64-mingw32-gcc
export CXX=i686-w64-mingw32-g++

./configure --prefix=/usr/i686-w64-mingw32 --host=i686-w64-mingw32 --disable-shared --enable-static LDFLAGS="-static"
make -j16
make install

```

Compile make-4.2
```bash
export CHOST=i686-w64-mingw32
export CC=i686-w64-mingw32-gcc
export CXX=i686-w64-mingw32-g++

./configure --host=i686-w64-mingw32 --prefix=/opt/kendryte-toolchain LDFLAGS="-static"
ln -s glob/glob.h .
make -j16
make install
```

Compile toolchain
```bash
export CHOST=i686-w64-mingw32

./configure --with-host=i686-w64-mingw32 --host=i686-w64-mingw32 --prefix=/opt/kendryte-toolchain --with-cmodel=medany --with-arch=rv64imafc --with-abi=lp64f LDFLAGS="-static"
make -j16
```


### Advanced Options

There are a number of additional options that may be passed to
configure.  See './configure --help' for more details.

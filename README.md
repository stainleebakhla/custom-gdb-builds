# Custom GDB Builds

These are some custom GDB builds built specifically for debugging remote Linux programs from a Windows computer. These GDB builds are made for running on a Windows host computer, and connect to a GDB Server running on a Linux target computer to debug a program running on the Linux computer.

## Getting Started

The builds provided in the repository can be used straight out of the box. The builds for each target system in present in the `usr` directory. The GDB executable is present in the `usr/bin` directory. Please check out other instructions on how to use GDB and GDB Server for remote debugging. The instructions provided here will describe the steps used to build these versions.

## Prerequisites

These gdb builds was built on a computer running Ubuntu 16.04. The following packages are required for building GDB

- gcc
- mingw-w64
- make
- flex
- bison
- texinfo

The following command will install all the dependencies

```
sudo apt-get install gcc mingw-w64 make flex bison texinfo
```

### Getting GDB Source Code

Since we will be building a custom version of GDB, we will be needing the source code to start from. GDB source code can be downloaded from [https://www.gnu.org/software/gdb/current/](https://www.gnu.org/software/gdb/current/). Alternatively GDB source can also be obtained from its Github page [https://github.com/bminor/binutils-gdb/tree/master/gdb](https://github.com/bminor/binutils-gdb/tree/master/gdb). I had used GDB version 7.8.0.20141029 for this build, and is available in the `downloads` folder in the repository.

### Getting Host information

Since we are making this build of GDB for Windows, there are two available options for specifying the host:

- `x86_64-w64-mingw32` - MinGW
- `x86_64-pc-cygwin` - Cygwin

This guide will use MinGW for further instructions.

### Getting Target Information

If GDB is already installed in the target system, we can use the following command to obtain the target build:

```
gdb --version
```

The above command will output some text onto the screen, among which there will be a line such as

```
This GDB was configured as "x86_64-linux-gnu"
```

which gives us the linux build. Some of the linux builds are given below, for which the GDB in this repository has been built:

- `x86_64-linux-gnu` - Ubuntu
- `x86_64-redhat-linux-gnu` - Red Hat Linux
- `x86_64-unknown-linux-gnu` - General Linux

## Building Instructions

These instructions will assume that the GDB source code has been downloaded into the `downloads` folder. The instructions for each of the build is given below.

### Ubuntu (x86_64-linux-gnu)

1. Extract the GDB sources

```
tar -xjf downloads/gdb-7.8.0.20141029.tar.bz2
```

2. Make a directory for saving the build

```
mkdir build-x86_64-w64-mingw32-x86_64-linux-gnu
```

3. Configure the build for Ubuntu

```
cd build-x86_64-w64-mingw32-x86_64-linux-gnu
../gdb-7.8.0.20141029/configure --host=x86_64-w64-mingw32 --target=x86_64-linux-gnu --disable-gdbserver --prefix=$(pwd)/usr
```

4. Build GDB

```
make
make install
```

5. Output

The output of the build will be available in the `usr` directory. The GDB executable will be available in the `usr/bin` directory. This build can now be copied onto a Windows computer for remoting debugging programs running on Ubuntu.

### Red Hat Linux (x86_64-redhat-linux-gnu)

The following commands will build GDB for remote debugging programs running on Red Hat.

```
tar -xjf downloads/gdb-7.8.0.20141029.tar.bz2
mkdir build-x86_64-w64-mingw32-x86_64-redhat-linux-gnu
cd build-x86_64-w64-mingw32-x86_64-redhat-linux-gnu
../gdb-7.8.0.20141029/configure --host=x86_64-w64-mingw32 --target=x86_64-redhat-linux-gnu --disable-gdbserver --prefix=$(pwd)/usr
make
make install
```

The output will be available in the `usr` directory. The GDB executable will be available in the `usr/bin` directory.

### General Linux (x86_64-unknown-linux-gnu)

The following commands will build GDB for remote debugging programs generally running on Linux.

```
tar -xjf downloads/gdb-7.8.0.20141029.tar.bz2
mkdir build-x86_64-w64-mingw32-x86_64-unknown-linux-gnu
cd build-x86_64-w64-mingw32-x86_64-unknown-linux-gnu
../gdb-7.8.0.20141029/configure --host=x86_64-w64-mingw32 --target=x86_64-unknown-linux-gnu --disable-gdbserver --prefix=$(pwd)/usr
make
make install
```

The output will be available in the `usr` directory. The GDB executable will be available in the `usr/bin` directory.

### All of the above

The following commands will build GDB for remote debugging programs running on any of the Linux platforms mentioned above.

```
tar -xjf downloads/gdb-7.8.0.20141029.tar.bz2
mkdir build-x86_64-w64-mingw32-x86_64-all-linux-gnu
cd build-x86_64-w64-mingw32-x86_64-all-linux-gnu
../gdb-7.8.0.20141029/configure --host=x86_64-w64-mingw32 --enable-targets=x86_64-linux-gnu,x86_64-unknown-linux-gnu,x86_64-redhat-linux-gnu --disable-gdbserver --prefix=$(pwd)/usr
make
make install
```

The output will be available in the `usr` directory. The GDB executable will be available in the `usr/bin` directory.

## Conclusion

These GDB builds have been provided in the hope that the pre-built GDBs may help someone for his requirement and also serve as a tutorial page for those wishing to create their own builds. The materials for this tutorial has been picked from the following blogs:

- [https://blog.jetbrains.com/clion/2016/11/clion-2016-3-eap-remote-gdb-debug-udl-rename-and-code-analysis-fixes/#remote_debug](https://blog.jetbrains.com/clion/2016/11/clion-2016-3-eap-remote-gdb-debug-udl-rename-and-code-analysis-fixes/#remote_debug)
- [https://youtrack.jetbrains.com/issue/CPP-8137](https://youtrack.jetbrains.com/issue/CPP-8137)
- [https://www.linux.com/news/remote-cross-target-debugging-gdb-and-gdbserver](https://www.linux.com/news/remote-cross-target-debugging-gdb-and-gdbserver)

Thank You

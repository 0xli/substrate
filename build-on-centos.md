### 1. libclang needed newer than 3.9.1
#### 1. error
```
error: failed to run custom build command for `librocksdb-sys v6.17.3`

Caused by:
  process didn't exit successfully: `/home/club/dot/substrate-node-template-05/target/release/build/librocksdb-sys-639b105735bcdfc6/build-script-build` (exit status: 101)
  --- stderr
  rocksdb/include/rocksdb/c.h:45:9: warning: #pragma once in main file, err: false
  thread 'main' panicked at '`libclang` function not loaded: `clang_Type_getNumTemplateArguments`. This crate requires that `libclang` 3.9 or later be installed on your system. For more information on how to accomplish this, see here: https://rust-lang.github.io/rust-bindgen/requirements.html#installing-clang-39', /home/club/.cargo/registry/src/github.com-1ecc6299db9ec823/clang-sys-1.2.0/src/lib.rs:1682:1
  note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
warning: build failed, waiting for other jobs to finish...
error: build failed

```
#### 1.2 install instruction from above message
https://rust-lang.github.io/rust-bindgen/requirements.html#installing-clang-39

https://clang.llvm.org/get_started.html

need gcc 5.1
#### 1.3 another tested method 
https://blog.csdn.net/qq_20309931/article/details/75291527
- upgrade cmake
```
  995  wget https://cmake.org/files/v3.21/cmake-3.21.1.tar.gz
  996  tar zxvf cmake-3.21.1.tar.gz 
  997  cd cmake-3.21.1/
  998  ./bootstrap 
  999  gmake -j$(nproc)
 1000  make install
 1001  sudo make install
 1002  cd ..
```
- install pip
```
 1012  sudo yum install epel-release
 1013  sudo yum install python-pip -y
 1014  sudo yum clean all
 1018  sudo pip install distribute
 1019  sudo pip install --upgrade pip
``` 
- install dependency
```
 1022  wget http://llvm.org/releases/3.9.1/llvm-3.9.1.src.tar.xz
 1023  wget http://llvm.org/releases/3.9.1/cfe-3.9.1.src.tar.xz
 1024  wget http://llvm.org/releases/3.9.1/compiler-rt-3.9.1.src.tar.xz
 1025  wget http://llvm.org/releases/3.9.1/clang-tools-extra-3.9.1.src.tar.xz
 1026  tar xvf llvm-3.9.1.src.tar.xz 
 1027  mv llvm-3.9.1.src  llvm
 1028  cd llvm/tools
 1029  tar xvf ../../cfe-3.9.1.src.tar.xz 
 1030  mv cfe-3.9.1.src clang
 1031  cd clang/tools
 1032  tar xvf ../../../../clang-tools-extra-3.9.1.src.tar.xz 
 1033  mv clang-tools-extra-3.9.1.src extra
 1034  cd ../../../projects/
 1035  tar xvf ../../compiler-rt-3.9.1.src.tar.xz 
 1036  mv compiler-rt-3.9.1.src compiler-rt
 1037  cd ../..
 1038  mkdir llvm-build
 1039  cd llvm-build/
 1040  cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/home/club/dot/clang3.9.1 -DLLVM_OPTIMIZED_TABLEGEN=1 ../llvm
 1041  make -j$(nproc)
 1042  make install
 ```
- compile substrate
``` 
 1043  cd ..
 1045  cd substrate-node-template-05/
 1046  cargo build --release
 1047  clang --version
 1048  set | grep LIBCLANG_PATH
 1049  find /home/club/dot/clang3.9.1/ -name libclang.so -print
 1050  export  LIBCLANG_PATH=/home/club/dot/clang3.9.1/lib/libclang.so
 ## or change the setting in .profile
 1063  source ~/.profile 
 1064  set | grep LIBCLANG_PATH
 1065  cargo build --release
```

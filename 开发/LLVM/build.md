
```shell
# 构建llvm
cmake \
  -S llvm-project/llvm \
  -B llvm-build \
  -G Ninja -DCMAKE_OSX_ARCHITECTURES='arm64;x86_64'  \
  -DCMAKE_BUILD_TYPE=Release \
  -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra;mlir;openmp;polly" \
  -DCMAKE_INSTALL_PREFIX=/Volumes/DATA/Runtimes/llvm \
  && cmake --build llvm-build --parallel \
  && cmake --install llvm-build 
```

```shell
cmake \
  -S llvm-project/llvm \
  -B llvm-build \
  -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra;mlir;openmp;polly" \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_CXX_COMPILER=`which clang++-11` \
  -DCMAKE_C_COMPILER=`which clang-11` \
  -DCMAKE_LINKER=`which lld-11` \
  &&  cmake --build llvm-build/ --parallel 12
```
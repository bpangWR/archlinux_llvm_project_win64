pkgname=llvm-project
pkgver=10.0.1
pkgrel=1
pkgdesc="build clang for WIN64"
arch=('x86_64')
url="https://clang.llvm.org/"
license=('custom:Apache 2.0 with LLVM Exception')
makedepends=('cmake' 'make' 'mingw-w64-gcc' 'python')

_source_base=https://github.com/llvm/llvm-project/releases/download/llvmorg-10.0.1
source=($_source_base/$pkgname-$pkgver.tar.xz{,.sig})

prepare() {
    cd "$srcdir/$pkgname-$pkgver"
    # do't link "-ldl" for WIN64
    sed -i "s/^find_library\(DL_LIBRARY_PATH dl\)/#find_library\(DL_LIBRARY_PATH dl\)/" clang/tools/libclang/CMakeLists.txt
    mkdir -p build
}

build() {
    cd "$srcdir/$pkgname-$pkgver/build"
    # makepkg would set these environment variables, but they are invalid for mingw-w64-gcc
    unset LDFLAGS CFLAGS CXXFLAGS CHOST CPPFLAGS COMMAND_MODE
    cmake ../llvm  -DCMAKE_C_COMPILER=/usr/bin/x86_64-w64-mingw32-gcc \
	  -DCMAKE_CXX_COMPILER=/usr/bin/x86_64-w64-mingw32-g++ \
	  -DCMAKE_SYSTEM_NAME=Windows \
	  -DCMAKE_RC_COMPILER=/usr/bin/x86_64-w64-mingw32-windres \
	  -DCMAKE_BUILD_TYPE=Release \
	  -DLLVM_ENABLE_PROJECTS=clang \
	  -DCMAKE_INSTALL_PREFIX=/WIN64/$pkgname-$pkgver \
	  -DCROSS_TOOLCHAIN_FLAGS_NATIVE="-DCMAKE_C_COMPILER=cc;-DCMAKE_CXX_COMPILER=c++"

    n=`lscpu | grep ^CPU\(s\): | cut -f2 -d':'`
    make -j$n
}

check() {
    cd "$srcdir/$pkgname-$pkgver/build"
    ls -l bin/clang.exe
}

package() {
    cd "$srcdir/$pkgname-$pkgver/build"
    DESTDIR="$pkgdir" make install
}

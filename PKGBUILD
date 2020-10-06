pkgname=llvm-project
pkgver=r12.012345
pkgrel=1
pkgdesc="build clang for WIN64"
arch=('x86_64')
url="https://clang.llvm.org/"
license=('custom:Apache 2.0 with LLVM Exception')
makedepends=('cmake' 'make' 'mingw-w64-gcc' 'python')

source=(git+https://github.com/llvm/llvm-project.git)


pkgver() {
  cd "$srcdir/llvm-project"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
    cd "$srcdir/llvm-project"
    mkdir -p build
}

build() {
    cd "$srcdir/llvm-project/build"
    # makepkg would set these environment variables, but they are not proper for cross-compiling using mingw-w64-gcc
    unset LDFLAGS CFLAGS CXXFLAGS CHOST CPPFLAGS COMMAND_MODE
    cmake ../llvm  -DCMAKE_C_COMPILER=/usr/bin/x86_64-w64-mingw32-gcc \
	  -DCMAKE_CXX_COMPILER=/usr/bin/x86_64-w64-mingw32-g++ \
	  -DCMAKE_SYSTEM_NAME=Windows \
	  -DCMAKE_RC_COMPILER=/usr/bin/x86_64-w64-mingw32-windres \
	  -DCMAKE_BUILD_TYPE=Release \
	  -DLLVM_ENABLE_PROJECTS=clang \
	  -DCMAKE_INSTALL_PREFIX=/WIN64/$pkgname-$pkgver \
	  -DCROSS_TOOLCHAIN_FLAGS_NATIVE="-DCMAKE_C_COMPILER=cc;-DCMAKE_CXX_COMPILER=c++"
    make -j "$(nproc)"
}

check() {
    cd "$srcdir/llvm-project/build"
    ls -l bin/clang.exe
}

package() {
    cd "$srcdir/llvm-project/build"
    DESTDIR="$pkgdir" make install
}

# Maintainer: Tsiry Sandratraina <tsiry.sndr@fluentci.io>

pkgname=rockbox-zig
pkgver=2025.02.16
pkgrel=1
pkgdesc="Open Source high quality audio player"
arch=('x86_64' 'aarch64')
url="https://github.com/tsirysndr/rockbox-zig"
license=('GPL-2.0')
depends=('sdl2' 'libunwind' 'alsa-lib')
makedepends=('git' 'rust' 'zig' 'sdl2' 'libunwind' 'rustup' 'cmake' 'protobuf' 'gcc13' 'make' 'autoconf' 'libtool' 'zip' 'deno' 'alsa-lib')
source=("git+https://github.com/tsirysndr/rockbox-zig.git#tag=$pkgver")
sha256sums=('SKIP')

prepare() {
    cd "$srcdir/$pkgname"
    git submodule update --init
}

build() {
    rm -rf /tmp/bin
    mkdir /tmp/bin
    export PATH=/tmp/bin:$PATH
    for bin in /usr/bin/*-13; do
      ln -s "$bin" "/tmp/bin/$(basename "$bin" -13)"
    done

    cd "$srcdir/$pkgname"
    cd webui/rockbox
    deno install
    deno run build

    rustup default stable

    cd "$srcdir/$pkgname"

    sed -i '/libfirmware\.linkSystemLibrary("usb");/d' build.zig || true

    mkdir -p build
    cd build
    ../tools/configure --target=sdlapp --type=N --lcdwidth=320 --lcdheight=240 --prefix=/usr
    TAG=$pkgver make zig -j$(nproc)
}

package() {
    cd "$srcdir/$pkgname/build"
    mkdir -p "$pkgdir/usr/bin"
    make PREFIX="$pkgdir/usr" install
    cp ../zig-out/bin/rockboxd "$pkgdir/usr/bin"
    cp ../target/release/rockbox "$pkgdir/usr/bin"
}

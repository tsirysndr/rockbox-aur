# Maintainer: Tsiry Sandratraina <tsiry.sndr@fluentci.io>

pkgname=rockbox-zig
pkgver=2025.01.18
pkgrel=1
pkgdesc="Open Source high quality audio player"
arch=('x86_64')
url="https://github.com/tsirysndr/rockbox-zig"
license=('GPL-2.0')
depends=('sdl2' 'libunwind')
makedepends=('git' 'rust' 'zig' 'sdl2' 'libunwind' 'rustup' 'cmake' 'protobuf' 'base-devel' 'zip' 'deno')
source=("git+https://github.com/tsirysndr/rockbox-zig.git#tag=$pkgver")
sha256sums=('SKIP')

prepare() {
    cd "$srcdir/$pkgname"
    git submodule update --init
}

build() {
    cd "$srcdir/$pkgname"
    cd webui/rockbox
    deno install
    deno run build

    rustup default stable

    cd "$srcdir/$pkgname"
    mkdir -p build
    cd build
    ../tools/configure --target=sdlapp --type=N --lcdwidth=320 --lcdheight=240 --prefix=/usr
    TAG=$pkgver make zig -j$(nproc)
}

package() {
    cd "$srcdir/$pkgname"
    make DESTDIR="$pkgdir/" install
    cp ../zig-out/bin/rockboxd "$pkgdir/usr/bin"
    cp ../target/release/rockbox "$pkgdir/usr/bin"
}

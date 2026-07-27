# Maintainer: Mircas001. <mircas001@protonmail.com>

pkgname=anadash-driver
pkgver=0.1.0
pkgrel=1
pkgdesc='This is the driver for the AnaDash!'
url='https://github.com/Mircas001/AnaDash'
license=('GPL-3.0-or-later')
makedepends=('cargo')
depends=('lm_sensors')
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
source=()
b2sums=()

_source_dir="repo/software/anadash-driver"

prepare() {
    msg2 "Performing sparse checkout..."
    git clone --filter=blob:none --no-checkout https://github.com/Mircas001/AnaDash.git "$srcdir/repo"

    cd "$srcdir/repo"
    git sparse-checkout init --cone
    git sparse-checkout set software
    git checkout main

    rm -rf "$srcdir/repo/software/firmware"
    
    cd "$srcdir/$_source_dir"
    export RUSTUP_TOOLCHAIN=stable
    cargo fetch --locked --target host-tuple
}

build() {
    cd "$srcdir/$_source_dir"
    export RUSTUP_TOOLCHAIN=stable
    export CARGO_TARGET_DIR=target
    cargo build --frozen --release --all-features
}

check() {
    true;
}

package() {
    install -Dm0755 -t "$pkgdir/usr/bin/" "$_source_dir/target/release/$pkgname"

    install -Dm644 "$_source_dir/99-anadash.rules"  "$pkgdir/usr/lib/udev/rules.d/99-anadash.rules"

    install -Dm644 "$_source_dir/anadash-driver@.service" "$pkgdir/usr/lib/systemd/system/anadash-driver@.service"
}

post_install() {
    udevadm control --reload-rules
    systemctl daemon-reload
}

post_upgrade() {
    post_install
}

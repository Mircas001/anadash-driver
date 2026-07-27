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

_package_dir="$srcdir/repo/software/firmware"

prepare() {
    _repo_url="https://github.com/Mircas001/AnaDash.git"
    msg2 "Performing sparse checkout..."

    git clone --filter=blob:none --no-checkout "$_repo_url" "$srcdir/repo"
    cd "$srcdir/repo"
    git sparse-checkout init --cone
    git sparse-checkout set software
    git checkout main

    cd "$_package_dir"
    export RUSTUP_TOOLCHAIN=stable
    cargo fetch --locked --target host-tuple
}

build() {
    cd "$_package_dir"
    export RUSTUP_TOOLCHAIN=stable
    export CARGO_TARGET_DIR=target
    cargo build --frozen --release --all-features
}

check() {
    true;
}

package() {
    install -Dm0755 -t "$pkgdir/usr/bin/" "$package_dir/target/release/$pkgname"

    install -Dm644 "$_package_dir/99-anadash.rules"  "$pkgdir/usr/lib/udev/rules.d/99-anadash.rules"

    install -Dm644 "$_package_dir/anadash-driver@.service" "$pkgdir/usr/lib/systemd/system/anadash-driver@.service"
}

# Maintainer: roehistat <mail at iyxeyl.me>

pkgname=kopa
pkgver=0.2.3
pkgrel=1
pkgdesc="TUI clipboard manager for Wayland"
arch=('x86_64')
url="https://github.com/eyenalxai/kopa"
license=('GPL3')
depends=('gcc-libs' 'sqlite' 'wayland')
makedepends=('cargo' 'bun')
options=('!debug' '!strip')
source=("$pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz"
  "kopa-daemon.service")
sha256sums=('7f8e00eb7923b67068145b18cf2eb50423f11643bd7f4c0a557df53921b66595'
            'd160090f5e87b0dc19d7034264ea43638b0d1fbf54cddb96d9134cee7199df8b')

prepare() {
  cd "$pkgname-$pkgver"
  export RUSTUP_TOOLCHAIN=stable

  cargo fetch --locked --target host-tuple --manifest-path kopa-daemon/Cargo.toml
  bun install --frozen-lockfile --production
}

build() {
  cd "$pkgname-$pkgver"
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target

  cargo build --frozen --release --manifest-path kopa-daemon/Cargo.toml
  bun build --compile --target=bun-linux-x64-modern ./tui/index.tsx --outfile ./dist/kopa
}

package() {
  cd "$pkgname-$pkgver"

  install -Dm0755 -t "$pkgdir/usr/bin/" "dist/kopa"
  install -Dm0755 -t "$pkgdir/usr/bin/" "target/release/kopa-daemon"
  strip --strip-unneeded "$pkgdir/usr/bin/kopa-daemon"
  install -Dm0644 -t "$pkgdir/usr/lib/systemd/user/" "$srcdir/kopa-daemon.service"
  install -Dm0644 -t "$pkgdir/usr/share/licenses/$pkgname/" LICENSE
}

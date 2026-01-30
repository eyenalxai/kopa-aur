# Maintainer: roehistat <mail at iyxeyl.me>

pkgname=kopa
pkgver=0.4.2
pkgrel=1
pkgdesc="TUI clipboard manager for Wayland"
arch=('x86_64')
url="https://github.com/eyenalxai/kopa"
license=('GPL3')
depends=('gcc-libs' 'sqlite' 'wayland')
makedepends=('bun')
options=('!debug' '!strip')
source=("$pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz"
  "kopa-daemon.service")
sha256sums=('2a11b7b07d01946e35297eb5c8d0afe1912246bab5616d498ea413b1cce09d34'
            '09c4f9c6ff8efc5a79469bfec4de070063bfafc4179ef1b95ac17edf718aa1f2')

prepare() {
  cd "$pkgname-$pkgver"
  bun install --frozen-lockfile --production
}

build() {
  cd "$pkgname-$pkgver"
  bun build --compile --target=bun-linux-x64-modern ./src/main.ts --outfile ./dist/kopa
}

package() {
  cd "$pkgname-$pkgver"

  # Install the compiled binary
  install -Dm0755 -t "$pkgdir/usr/bin/" "dist/kopa"

  # Copy sharp's native modules (required for bun compile bug workaround)
  # See: https://github.com/oven-sh/bun/issues/15374
  install -d "$pkgdir/usr/lib/kopa/node_modules/@img"
  cp -r "node_modules/@img/sharp-libvips-linux-x64" "$pkgdir/usr/lib/kopa/node_modules/@img/"
  cp -r "node_modules/@img/sharp-linux-x64" "$pkgdir/usr/lib/kopa/node_modules/@img/"

  # Copy sharp dependencies
  cp -r "node_modules/@img/colour" "$pkgdir/usr/lib/kopa/node_modules/@img/"

  # Copy sharp package itself
  cp -r "node_modules/sharp" "$pkgdir/usr/lib/kopa/node_modules/"

  # Copy sharp's JS dependencies
  cp -r "node_modules/detect-libc" "$pkgdir/usr/lib/kopa/node_modules/"
  cp -r "node_modules/semver" "$pkgdir/usr/lib/kopa/node_modules/"

  install -Dm0644 -t "$pkgdir/usr/lib/systemd/user/" "$srcdir/kopa-daemon.service"
  install -Dm0644 -t "$pkgdir/usr/share/licenses/$pkgname/" LICENSE
}

# Maintainer: Till Faelligen <tfaelligen at gmail dot com>
pkgname='matrix-conduit-git'
_pkgname='conduit'
pkgver=0.3.0.1404.g6788225
pkgrel=1
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
url='https://conduit.rs'
pkgdesc='A Matrix homeserver written in Rust'
license=('Apache')
depends=('gcc-libs')
makedepends=('rust' 'cargo' 'git' 'clang')
provides=('conduit')
source=(
  "$_pkgname::git+https://gitlab.com/famedly/conduit"
  'install-script.install'
  '0001-update-service-dynamicuser_paths.patch'
  '0002-example-info.patch'
)
backup=(
  'etc/matrix-conduit/conduit.toml'
)
install=install-script.install
sha256sums=(
  'SKIP'
  '085e69c4589597f0b58509f72f04ecc497aaddcc5de6138f4ae5009f0d0cf4c9'
  '88ff8d11d9617bc7af8025452d89231ae84f80605da3f183acf90bfddf57feda'
  'c9f63b25049e51688911d8ec816fe5676d3cc70d889c321891e6f27888973ce9'
)

prepare() {
  cd "$_pkgname"
  patch --forward --strip=1 --input="${srcdir}/0001-update-service-dynamicuser_paths.patch"
  patch --forward --strip=1 --input="${srcdir}/0002-example-info.patch"
}

pkgver() {
	cd $_pkgname/
	echo "$(grep '^version =' Cargo.toml|head -n1|cut -d\" -f2|cut -d\- -f1).$(git rev-list --count HEAD).g$(git rev-parse --short HEAD)"
}

build(){
  cd "$_pkgname"
  env CARGO_INCREMENTAL=0 cargo build --release --locked
}

package() {
  cd $_pkgname
  install -D -m755 target/release/conduit "$pkgdir/usr/bin/matrix-conduit"
  install -D -m0644 conduit-example.toml "$pkgdir/etc/matrix-conduit/conduit.toml"
  install -D -m0644 conduit-example.toml "$pkgdir/usr/share/doc/matrix-conduit/conduit-example.toml"
  install -D -m0644 debian/matrix-conduit.service "$pkgdir/usr/lib/systemd/system/matrix-conduit.service"
}

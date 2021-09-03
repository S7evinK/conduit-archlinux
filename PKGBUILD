# Maintainer: Till Faelligen <tfaelligen at gmail dot com>
pkgname='matrix-conduit-git'
_pkgname='conduit'
pkgver=0.2.0.1040.g73b7643
pkgrel=1
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
url='https://conduit.rs'
pkgdesc='A Matrix homeserver written in Rust'
license=('Apache')
depends=('gcc-libs')
makedepends=('rust' 'cargo' 'git')
provides=('conduit')
source=(
  "$_pkgname::git+https://gitlab.com/famedly/conduit"
  'install-script.install'
  '0001-update-service-dynamicuser_paths.patch'
)
backup=(
  'etc/matrix-conduit.toml'
)
install=install-script.install
sha256sums=(
  'SKIP'
  '476c38accc6c3351805bdab4e24fc5a296c10dad6f59842c49b07aec31966307'
  'aec20a57463f90e151af2ff1461caebaae070d8daea0a3c587064ccbe0724ce9'
)

prepare() {
  cd "$_pkgname"
  patch --forward --strip=1 --input="${srcdir}/0001-update-service-dynamicuser_paths.patch"
  echo -e "# THIS IS AN EXAMPLE CONFIGURATION WHICH NEEDS TO BE UPDATED.\n# See /usr/share/doc/matrix-conduit/conduit-example.toml for further configuration.\n" > tmpfile
  cat tmpfile conduit-example.toml > conduit-example_new.toml && mv conduit-example_new.toml conduit-example.toml && rm tmpfile
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
  install -D -m0644 conduit-example.toml "$pkgdir/etc/matrix-conduit.toml"
  install -D -m0644 conduit-example.toml "$pkgdir/usr/share/doc/matrix-conduit/conduit-example.toml"
  install -D -m0644 debian/matrix-conduit.service "$pkgdir/usr/lib/systemd/system/matrix-conduit.service"
}

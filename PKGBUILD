# This PKGBUILD is the bare minimum to build mongodb core on a Raspberry Pi 4,
# which is armv7l (or what debian calls armhf).
#
# Made by Mikey Dickerson <pomonamikey@gmail.com>, borrowing heavily from
# the package by Ali Molaei at https://aur.archlinux.org/mongodb-bin-3.6.git

pkgname="mongodb"
pkgver="3.2.22"
pkgrel="3"
pkgdesc="An open source, schema-free document-oriented database"
arch=("armv7h")
url="https://www.mongodb.com/"
license=("AGPL")
provides=("mongodb=$pkgver")
conflicts=("mongodb" "mongodb-bin")
depends=("boost-libs"
         "snappy"
         "zlib"
         "libstemmer"
         "pcre")
makedepends=("boost"
             "python2")
source=(
    "https://github.com/ddcc/mongodb/archive/v3.2.22-2.tar.gz"
    "ddcc_arm_build.patch"
    "stdlib_map.patch"
    "disable_libstdcc_check.patch"
    "mongodb.service"
    "mongodb.conf"
    "mongodb.sysusers"
    "mongodb.tmpfiles")

sha256sums=(
    "0c6dcf604fb40ae737ee2397fd5cfd1ea05c551076d0ea4d66ac728e1637bafc"
    "568090738bc0c76e40ce4578bb03b549c5d4f07a1d96ef9b405514c634a85d7e"
    "316880315c7e7cff3b12ca3d75b02ef7c5dd724b6759cec21922f0c6d277cd40"
    "f8b6d51ef55609f10c424daad2815335c364895a4d7f55be2f51d8866f0420ea"
    "9309ffff8a59d112e877da2909c92edfe821d5370660ac274c046df82e4f4c5e"
    "8010ce728d657524cd76b5afda7ffbc1cc389642336b12b89cec5df2b09fc0e4"
    "47b884569102f7c79017ee78ef2e98204a25aa834c0ee7d5d62c270ab05d4e2b"
    "51ee1e1f71598aad919db79a195778e6cb6cfce48267565e88a401ebc64497ac")

backup=("etc/mongodb.conf")

scons_options="--use-system-pcre --use-system-boost --use-system-snappy --use-system-zlib --use-system-valgrind --use-system-stemmer --disable-warnings-as-errors --allocator=system --wiredtiger=off --mmapv1=on"

# can't say ${pkgname}-${pkgversion} because the version is not allowed to
# have the -2 in it.
builddir="mongodb-3.2.22-2"

prepare() {
  cd "${builddir}"
  patch --forward --strip=1 --input="${srcdir}/ddcc_arm_build.patch"
  patch --forward --strip=1 --input="${srcdir}/stdlib_map.patch"
  patch --forward --strip=1 --input="${srcdir}/disable_libstdcc_check.patch"
}

build() {
  cd "${srcdir}/${builddir}"
  python2 src/third_party/scons-2.5.0/scons.py ${scons_options} core
  strip "${srcdir}/${builddir}/mongo"
  strip "${srcdir}/${builddir}/mongod"
  strip "${srcdir}/${builddir}/mongos"
}

package() {
  mkdir -p "${pkgdir}/usr/bin"
  install -Dm755 "${srcdir}/${builddir}/mongo"  "$pkgdir/usr/bin/mongo"
  install -Dm755 "${srcdir}/${builddir}/mongod" "$pkgdir/usr/bin/mongod"
  install -Dm755 "${srcdir}/${builddir}/mongos" "$pkgdir/usr/bin/mongos"
  install -Dm644 "$srcdir/mongodb.conf" "$pkgdir/etc/mongodb.conf"
  install -Dm644 "$srcdir/mongodb.service" "$pkgdir/usr/lib/systemd/system/mongodb.service"
  install -Dm644 "$srcdir/mongodb.sysusers" "$pkgdir/usr/lib/sysusers.d/mongodb.conf"
  install -Dm644 "$srcdir/mongodb.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/mongodb.conf"
}

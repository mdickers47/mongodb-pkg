# This PKGBUILD is the bare minimum to build mongodb core on a Raspberry Pi 4,
# which is armv7l (or what debian calls armhf).
#
# Made by Mikey Dickerson <pomonamikey@gmail.com>, borrowing heavily from
# the package by Ali Molaei at https://aur.archlinux.org/mongodb-bin-3.6.git

pkgname="mongodb-bin-3.2"
pkgver="3.2.22"
pkgrel="1"
pkgdesc="An open source, schema-free document-oriented database"
arch=("armv7h")
url="https://www.mongodb.com/"
license=("AGPL")
provides=("mongodb=$pkgver")
conflicts=("mongodb" "mongodb-bin")
makedepends=("pcre"
	     "boost"
             "snappy"
             "zlib"
             "valgrind"
             "libstemmer"
             "python2")
source=(
    "https://github.com/mdickers47/mongodb/archive/master.tar.gz"
    "mongodb.service"
    "mongodb.conf"
    "mongodb.sysusers"
    "mongodb.tmpfiles")

sha256sums=(
    "e483640cf18b954595d9f886a614c21e57095c0fcf0078069e4ed74826ce1fa2"
    "9309ffff8a59d112e877da2909c92edfe821d5370660ac274c046df82e4f4c5e"
    "8010ce728d657524cd76b5afda7ffbc1cc389642336b12b89cec5df2b09fc0e4"
    "47b884569102f7c79017ee78ef2e98204a25aa834c0ee7d5d62c270ab05d4e2b"
    "51ee1e1f71598aad919db79a195778e6cb6cfce48267565e88a401ebc64497ac")

backup=("etc/mongodb.conf")

scons_options="--use-system-pcre --use-system-boost --use-system-snappy --use-system-zlib --use-system-valgrind --use-system-stemmer --disable-warnings-as-errors --allocator=system --wiredtiger=off --mmapv1=on"

prepare() {
  cd "${srcdir}/mongodb-master"
  #python2 src/third_party/scons-2.5.0/scons.py ${scons_options} core
  strip "$srcdir/mongodb-master/mongo"
  strip "$srcdir/mongodb-master/mongod"
  strip "$srcdir/mongodb-master/mongos"
}

package() {
  mkdir -p "${pkgdir}/usr/bin"
  install -Dm755 "$srcdir/mongodb-master/mongo"  "$pkgdir/usr/bin/mongo"
  install -Dm755 "$srcdir/mongodb-master/mongod" "$pkgdir/usr/bin/mongod"
  install -Dm755 "$srcdir/mongodb-master/mongos" "$pkgdir/usr/bin/mongos"
  install -Dm644 "$srcdir/mongodb.conf" "$pkgdir/etc/mongodb.conf"
  install -Dm644 "$srcdir/mongodb.service" "$pkgdir/usr/lib/systemd/system/mongodb.service"
  install -Dm644 "$srcdir/mongodb.sysusers" "$pkgdir/usr/lib/sysusers.d/mongodb.conf"
  install -Dm644 "$srcdir/mongodb.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/mongodb.conf"
}

# Mikey Dickerson <pomonamikey@gmail.com>

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
    "https://github.com/mdickers47/mongodb/master.zip"
    "mongodb.service"
    "mongodb.conf"
    "mongodb.sysusers"
    "mongodb.tmpfiles")

sha256sums=("154f01386f37ef798b42ec0a573c8e73b1bb79074bbfa7e9ff68a8d02d406c90"
            "13e251236a4127478bd47b3f581bed436e5669988b38e541ab8a6a264f2b8a94"            
            "12511ad0a6158871982d793b505fed3755fbc0577fc0ea7e47632828e0b6a8a1"
            "19f55ab28652b3817e98fc3f15cc2f6f3255a5e1dfd7b0d5a27c9ba22fd2703e"
            "8010ce728d657524cd76b5afda7ffbc1cc389642336b12b89cec5df2b09fc0e4"
            "47b884569102f7c79017ee78ef2e98204a25aa834c0ee7d5d62c270ab05d4e2b"
            "51ee1e1f71598aad919db79a195778e6cb6cfce48267565e88a401ebc64497ac"
            "09d99ca61eb07873d5334077acba22c33e7f7d0a9fa08c92734e0ac8430d6e27")

backup=("etc/mongodb.conf")

scons_options="--use-system-pcre --use-system-boost --use-system-snappy --use-system-zlib --use-system-valgrind --use-system-stemmer --disable-warnings-as-errors --allocator=system --wiredtiger=off --mmapv1=on"

prepare() {
  cd "${srcdir}"
  tar xf master.tar.gz
  cd mongodb-master
  python2 third_party/scons-2.5.0/scons.py ${scons_options} core
  strip "$srcdir/mongo"
  strip "$srcdir/mongod"
  strip "$srcdir/mongos"
}

package() {
  mkdir -p "${pkgdir}/usr/bin"
  install -Dm755 "$srcdir/mongo"  "$pkgdir/usr/bin/mongo"
  install -Dm755 "$srcdir/mongod" "$pkgdir/usr/bin/mongod"
  install -Dm755 "$srcdir/mongos" "$pkgdir/usr/bin/mongos"
  install -Dm644 "$srcdir/mongodb.conf" "$pkgdir/etc/mongodb.conf"
  install -Dm644 "$srcdir/mongodb.service" "$pkgdir/usr/lib/systemd/system/mongodb.service"
  install -Dm644 "$srcdir/mongodb.sysusers" "$pkgdir/usr/lib/sysusers.d/mongodb.conf"
  install -Dm644 "$srcdir/mongodb.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/mongodb.conf"
}

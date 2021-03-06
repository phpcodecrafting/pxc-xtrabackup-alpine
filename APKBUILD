# Contributor: Peter Szalatnay <theotherland@gmail.com>
# Maintainer: Peter Szalatnay <theotherland@gmail.com>
pkgname=percona-xtrabackup
pkgver=2.4.6
pkgrel=0
pkgdesc='Percona XtraBackup is a MySQL non-blocking backup solution for InnoDB and XtraDB data'
arch=all
url='http://www.percona.com/software/percona-xtrabackup/'
license='GPL'
makedepends='bash bison cmake ncurses-dev libaio-dev libgcrypt-dev zlib-dev curl-dev libev-dev vim'
source="https://github.com/percona/percona-xtrabackup/archive/percona-xtrabackup-$pkgver.tar.gz
        fix-posix_timers.patch
        "

subpackages="$pkgname-doc $pkgname-test:mytest
        "

_builddir="$srcdir/percona-xtrabackup-percona-xtrabackup-$pkgver"
prepare() {
        local i
        cd "$_builddir"
        for i in $source; do
                case $i in
                *.patch) msg $i; patch -p1 -i "$srcdir"/$i || return 1;;
                esac
        done
}

build() {
        cd "$_builddir"

        local CFLAGS="-DSIGEV_THREAD_ID=4"

        cmake . -DBUILD_CONFIG=xtrabackup_release \
                -DCMAKE_INSTALL_PREFIX=/usr \
                -DWITH_MAN_PAGES=OFF \
                -DDOWNLOAD_BOOST=1 \
                -DWITH_BOOST="$_builddir"/libboost \
                || return 1

        make || return 1
}

package() {
        cd "$_builddir"
        make DESTDIR="$pkgdir/" install || return 1

        install -Dm644 COPYING "$pkgdir"/usr/share/licenses/$pkgname/COPYING || return 1
}

mytest() {
    pkgdesc="The test suite distributed with Percona XtraBackup"
    mkdir -p "$subpkgdir"/usr/bin || return 1
    mv "$pkgdir"/usr/xtrabackup-test \
        "$subpkgdir"/usr/bin/ \
        || return 1
}

md5sums="6e6c4a36dbac6560579d34d6e8ffad43  percona-xtrabackup-2.4.6.tar.gz
bae0925c27db3c154b7749fe5737bd1d  fix-posix_timers.patch"
sha256sums="b7cac3b7dae428f3ff03c8d73d19b9ef9dc880c5b860b652f5cc4ca71970d5ba  percona-xtrabackup-2.4.6.tar.gz
2c3fdccadf30fb3f1258663efaf5c60526236b56463f8d29c442107ac537488f  fix-posix_timers.patch"
sha512sums="99e27f2615d8d6b0742cfa2ecc29174d13750eaadfec7f4035abf49832ee4f747c4a5e13ef8b8e72b86236d9557096bd3b47212f6d5442282d9394f058d17822  percona-xtrabackup-2.4.6.tar.gz
707c4b5e083326a173481764a275d2824fae4a88935227d38baa64d42312fdf1e6f7eae877c335c2744ea67c5fe128dafceed1f5af4dd0913a93b2f7269a00b9  fix-posix_timers.patch"
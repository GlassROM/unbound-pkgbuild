# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Hisato Tatekura <hisato_tatekura@excentrics.net>
# Contributor: Massimiliano Torromeo <massimiliano DOT torromeo AT google mail service>

pkgname=unbound
pkgver=1.4.10
pkgrel=1
pkgdesc='Validating, recursive, and caching DNS resolver'
arch=('i686' 'x86_64')
url='http://unbound.net/'
license=('custom:BSD')
options=('!libtool')
depends=('openssl' 'ldns')
makedepends=('expat')
optdepends=('expat: unbound-anchor')
backup=('etc/unbound/unbound.conf')
source=("http://unbound.net/downloads/$pkgname-$pkgver.tar.gz"
        'unbound.conf'
        'rc.d')
sha1sums=('ac9ab61a51e147ade69ca8b043fee2ed76336a62'
          '5d473ec2943fd85367cdb653fcd58e186f07383f'
          'a0c8c496d71d43ed9e09b170d3df836dfb096480')

install=install

build() {
	cd "$srcdir/$pkgname-$pkgver"
	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--localstatedir=/var \
		--enable-static=no \
		--disable-rpath \
		--with-conf-file=/etc/unbound/unbound.conf \
		--with-pidfile=/var/run/unbound.pid
	make
}

package() {
	cd "$srcdir/$pkgname-$pkgver"
	make DESTDIR="$pkgdir" install
	install -D -m644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
	install -D -m755 ../rc.d "$pkgdir/etc/rc.d/$pkgname"
	install -D -m644 ../unbound.conf "$pkgdir/etc/unbound/unbound.conf"
	install -D -m644 doc/example.conf.in "$pkgdir/etc/unbound/unbound.conf.example"
}

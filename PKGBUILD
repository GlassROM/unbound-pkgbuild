# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Hisato Tatekura <hisato_tatekura@excentrics.net>
# Contributor: Massimiliano Torromeo <massimiliano DOT torromeo AT google mail service>

pkgname=unbound
pkgver=1.4.20
pkgrel=2
pkgdesc='Validating, recursive, and caching DNS resolver'
url='http://unbound.net/'
license=('custom:BSD')
arch=('i686' 'x86_64')
depends=('openssl' 'ldns')
makedepends=('expat')
optdepends=('expat: unbound-anchor')
backup=('etc/unbound/unbound.conf')
source=("http://unbound.net/downloads/${pkgname}-${pkgver}.tar.gz"
        'service'
        'conf')
sha1sums=('1752976533be2a4f0c9cdbab9d2cbb67d4f27c43'
          'b543ae6f8b87423bec095fca6b335a9ee43739a8'
          '5d473ec2943fd85367cdb653fcd58e186f07383f')

options=('!libtool')
install=install

build() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--localstatedir=/var \
		--sbindir=/usr/bin \
		--disable-static \
		--disable-rpath \
		--with-conf-file=/etc/unbound/unbound.conf \
		--with-pidfile=/run/unbound.pid
	make
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install
	install -Dm644 doc/example.conf.in "${pkgdir}/etc/unbound/unbound.conf.example"
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 ../service "${pkgdir}/usr/lib/systemd/system/unbound.service"
	install -Dm644 ../conf "${pkgdir}/etc/unbound/unbound.conf"
}

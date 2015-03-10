# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Hisato Tatekura <hisato_tatekura@excentrics.net>
# Contributor: Massimiliano Torromeo <massimiliano DOT torromeo AT google mail service>

pkgname=unbound
pkgver=1.5.3
pkgrel=1
pkgdesc='Validating, recursive, and caching DNS resolver'
url='http://unbound.net/'
license=('custom:BSD')
arch=('i686' 'x86_64')
makedepends=('expat')
optdepends=('expat: unbound-anchor')
depends=('openssl' 'ldns' 'libevent' 'dnssec-anchors')
backup=('etc/unbound/unbound.conf')
source=("http://unbound.net/downloads/${pkgname}-${pkgver}.tar.gz"
        'service'
        'conf')
sha1sums=('9ae0d8270df4591559d54ee4d61c550526521ca3'
          '63fcc187cec6f262d81600e66c6747285c72ad15'
          '98515708441cb831890a0b6d1986fd40649646c0')

install=install

build() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--localstatedir=/var \
		--sbindir=/usr/bin \
		--disable-rpath \
		--with-libevent \
		--with-rootkey-file=/etc/trusted-key.key \
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

# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Hisato Tatekura <hisato_tatekura@excentrics.net>
# Contributor: Massimiliano Torromeo <massimiliano DOT torromeo AT google mail service>

pkgname=unbound
pkgver=1.6.1
pkgrel=1
pkgdesc='Validating, recursive, and caching DNS resolver'
url='https://unbound.net/'
license=('custom:BSD')
arch=('i686' 'x86_64')
makedepends=('expat')
optdepends=('expat: unbound-anchor')
depends=('openssl' 'ldns' 'libevent' 'dnssec-anchors')
backup=('etc/unbound/unbound.conf')
validpgpkeys=('EDFAA3F2CA4E6EB05681AF8E9F6F1C2D7E045F8D')
source=("https://unbound.net/downloads/${pkgname}-${pkgver}.tar.gz"{,.asc}
        'service'
        'hook'
        'conf')
sha1sums=('41369fcfd37844b02b7293b37ec78e69f0db34c7' 'SKIP'
          'b543ae6f8b87423bec095fca6b335a9ee43739a8'
          '098d680a06e730330e3ccbdd58234d07ad1837dc'
          '98515708441cb831890a0b6d1986fd40649646c0')

install=install

build() {
	cd "${srcdir}/${pkgname}-${pkgver}"

	# Build against embedded flex instead of system one, see:
	# https://www.nlnetlabs.nl/bugs-script/show_bug.cgi?id=1223
	export LEX=:

	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--localstatedir=/var \
		--sbindir=/usr/bin \
		--disable-rpath \
		--enable-pie \
		--enable-relro-now \
		--with-conf-file=/etc/unbound/unbound.conf \
		--with-pidfile=/run/unbound.pid \
		--with-rootkey-file=/etc/trusted-key.key \
		--with-libevent \

	make
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install
	install -Dm644 doc/example.conf.in "${pkgdir}/etc/unbound/unbound.conf.example"
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 ../service "${pkgdir}/usr/lib/systemd/system/unbound.service"
	install -Dm644 ../hook "${pkgdir}/usr/share/libalpm/hooks/unbound-key.hook"
	install -Dm644 ../conf "${pkgdir}/etc/unbound/unbound.conf"
}

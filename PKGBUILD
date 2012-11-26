# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Hisato Tatekura <hisato_tatekura@excentrics.net>
# Contributor: Massimiliano Torromeo <massimiliano DOT torromeo AT google mail service>

pkgname=unbound
pkgver=1.4.18
pkgrel=3
pkgdesc='Validating, recursive, and caching DNS resolver'
url='http://unbound.net/'
license=('custom:BSD')
arch=('i686' 'x86_64')
options=('!libtool')
depends=('openssl' 'ldns')
makedepends=('expat')
optdepends=('expat: unbound-anchor')
backup=('etc/unbound/unbound.conf')
source=("http://unbound.net/downloads/${pkgname}-${pkgver}.tar.gz"
        'unbound.service'
        'unbound.conf'
        'rc.d')
sha1sums=('b64b4c9f7981df4e7589ebb770a31352a09db3fb'
          '5bc313cd978e4d6efe8c13600e838c70629be477'
          '5d473ec2943fd85367cdb653fcd58e186f07383f'
          'dc96e772f467b32555df21d16fdb15e98194c228')

install=install

build() {
	cd "${srcdir}/${pkgname}-${pkgver}"

	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--localstatedir=/var \
		--enable-static=no \
		--disable-rpath \
		--with-conf-file=/etc/unbound/unbound.conf \
		--with-pidfile=/run/unbound.pid \

	make
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"

	make DESTDIR="${pkgdir}" install

	install -D -m644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -D -m755 ../rc.d "${pkgdir}/etc/rc.d/${pkgname}"
	install -D -m644 ../unbound.conf "${pkgdir}/etc/unbound/unbound.conf"
	install -D -m644 doc/example.conf.in "${pkgdir}/etc/unbound/unbound.conf.example"
	install -D -m644 ../unbound.service "${pkgdir}/usr/lib/systemd/system/unbound.service"
}

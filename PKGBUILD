# Maintainer: David Runge <dvzrv@archlinux.org>
# Maintainer: Bruno Pagani <archange@archlinux.org>
# Maintainer: T.J. Townsend <blakkheim@archlinux.org>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Hisato Tatekura <hisato_tatekura@excentrics.net>
# Contributor: Massimiliano Torromeo <massimiliano DOT torromeo AT google mail service>

pkgname=unbound
pkgver=1.19.2
pkgrel=1
pkgdesc="Validating, recursive, and caching DNS resolver"
arch=(x86_64)
url="https://unbound.net/"
license=(BSD-3-Clause)
depends=(
  dnssec-anchors
  fstrm
  glibc
  hiredis
  ldns
  libnghttp2
  libsodium
)
makedepends=(
  expat
  libevent
  openssl
  protobuf-c
  python
  swig
  systemd
)
optdepends=(
  'expat: for unbound-anchor'
  'sh: for unbound-control-setup'
  'python: for python-bindings'
)
provides=(libunbound.so)
backup=(etc/$pkgname/$pkgname.conf)
source=(
  https://unbound.net/downloads/$pkgname-$pkgver.tar.gz{,.asc}
  $pkgname-1.14.0-trust_anchor_file.patch
  $pkgname-sysusers.conf
  $pkgname-tmpfiles.conf
  $pkgname-trusted-key.hook
)
sha512sums=('03183f9d52df5644808d7cbbf2d15458a2cf5bf79bd952bbd4384bcef2e6899631605ce7780700169d7532cec0203c16765bb7706e3717241300904763914350'
            'SKIP'
            '9590d3d459d96f99cbc7482fae0f5318dd22a034e45cff18079e4f3c9f9c3c1d7af90cdd5353fb469eac08c535555fd164097b496286b807b2117e8a3a6cd304'
            'ef71d4e9b0eb0cc602d66bd0573d9424578fe33ef28a852c582d56f0fd34fdd63046c365ef7aed8b84a461b81254240af7ad3fd539da72f9587817d21bd6c585'
            '6b1849ae9d7cf427f6fa6cd0590e8f8c3f06210d2d6795e543b0f325a9e866db0f5db2275a29fa90f688783c0dd16f19c8a49a9817d5f5444e13f8f2df3ff712'
            '613826cdf5ab6e77f2805fa2aa65272508dcd11090add1961b3df6dfac3b67db016bc9f45fbcf0ef0de82b2d602c153d5263a488027a6cf13a72680b581b266d')
b2sums=('f8dcee649e5e1dfaab9285964419b4d957f0035e484021e3131784512fed842ee46c25d7b47304aca4f03a0480877b939968bca22e80620434d1d2cb7013c9b6'
        'SKIP'
        '0978ab5c0474ed29de9c0904a46d114413e094dafeadaac4f10cdbc19e4152fcc064d7cdb8c331da7c2531075aa699326b84e21da1a8218a6f00a10f0e107b3d'
        '292a3c2e5fde292a03b6c9b2ddabd5089f52e73b50a404c3d9f54c1a43184924b661a21eea61cc521c594c1005a3b40b630fa585a38195c61298f9b24b248b92'
        'd3951006b43068be904c6b91a9e0563d56228225854e12b40abbdd4ba9b47338e97265837297a6de879acbc8051bb749163f9457683f5e12fc29ac2e7b687fd3'
        'd28785390eb6c125bd26ca11f097fe8864b080482157deeb7c70e9bee47ff2844abaed574db59a7c152ed3ec0acba05cfee4c3751f7a9f553320b064578f86c7')
validpgpkeys=(EDFAA3F2CA4E6EB05681AF8E9F6F1C2D7E045F8D) # W.C.A. Wijngaards <wouter@nlnetlabs.nl>

prepare() {
  # enable trusted-anchor-file and set it to an unbound specific location
  patch -p1 -d $pkgname-$pkgver -i ../$pkgname-1.14.0-trust_anchor_file.patch
  cd $pkgname-$pkgver
  autoreconf -fiv
}

build() {
  local configure_options=(
    --prefix=/usr
    --sysconfdir=/etc
    --localstatedir=/var
    --sbindir=/usr/bin
    --disable-rpath
    --enable-dnscrypt
    --enable-dnstap
    --enable-pie
    --enable-relro-now
    --enable-subnet
    --enable-systemd
    --enable-tfo-client
    --enable-tfo-server
    --enable-cachedb
    --with-libhiredis
    --with-conf-file=/etc/unbound/unbound.conf
    --with-pidfile=/run/unbound.pid
    --with-rootkey-file=/etc/trusted-key.key
    --with-libevent
    --with-libnghttp2
    --with-pyunbound
  )

  cd $pkgname-$pkgver
  ./configure "${configure_options[@]}"
  # prevent excessive overlinking due to libtool
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool
  make
}

check() {
  make -k check -C $pkgname-$pkgver
}

package() {
  depends+=(
    libevent libevent-2.1.so
    openssl libcrypto.so libssl.so
    protobuf-c libprotobuf-c.so
    systemd-libs libsystemd.so
  )

  make DESTDIR="$pkgdir" install -C $pkgname-$pkgver
  install -vDm 644 $pkgname-$pkgver/contrib/$pkgname.service -t "$pkgdir/usr/lib/systemd/system/"
  install -vDm 644 $pkgname-$pkgver/LICENSE -t "$pkgdir/usr/share/licenses/$pkgname/"
  install -vDm 644 $pkgname-sysusers.conf "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
  install -vDm 644 $pkgname-tmpfiles.conf "$pkgdir/usr/lib/tmpfiles.d/$pkgname.conf"
  # libalpm hook to copy the dnssec-anchors provided key to /etc/unbound
  install -vDm 644 unbound-trusted-key.hook -t "$pkgdir/usr/share/libalpm/hooks/"
}

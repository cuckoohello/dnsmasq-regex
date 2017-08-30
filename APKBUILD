# Maintainer: Yafeng Shan <yafeng.shan@gmail.com>
pkgname=dnsmasq-regex
pkgver=2.78
pkgrel=1
pkgdesc="A lightweight DNS, DHCP, RA, TFTP and PXE server with dnssec and regex support"
url="https://github.com/cuckoohello/dnsmasq-regex"
arch="all"
license="GPL2"
depends="!dnsmasq-dnssec !dnsmasq"
makedepends="linux-headers nettle-dev pcre-dev"
install="$pkgname.pre-install $pkgname.pre-upgrade"
source="$pkgname-$pkgver.zip::https://github.com/cuckoohello/dnsmasq-regex/archive/$pkgver.zip
	dnsmasq.initd
	dnsmasq.confd
	uncomment-conf-dir.patch
	"
builddir="$srcdir/$pkgname-$pkgver"

build() {
	cd "$builddir"

	make CFLAGS="$CFLAGS" COPTS="-DHAVE_DNSSEC -DHAVE_REGEX" all || return 1
}

# dnsmasq doesn't provide any test suite (shame on them!), so just check that
# the binary isn't totally broken...
check() {
	cd "$builddir"
	./src/dnsmasq --help >/dev/null
}

package() {
	cd "$builddir"

        install -D -m 755 src/dnsmasq \
                "$pkgdir"/usr/sbin/dnsmasq || return 1

	install -D -m755 "$srcdir"/dnsmasq.initd \
		"$pkgdir"/etc/init.d/dnsmasq || return 1

	install -D -m644 "$srcdir"/dnsmasq.confd \
		"$pkgdir"/etc/conf.d/dnsmasq || return 1

	install -m644 dnsmasq.conf.example "$pkgdir"/etc/dnsmasq.conf || return 1
	install -d -m755 "$pkgdir"/etc/dnsmasq.d

	install -D -m 644 trust-anchors.conf \
		"$pkgdir"/usr/share/$pkgname/trust-anchors.conf || return 1
}


sha512sums="9983cce82eb5f0c273badbda3022afa4e89f1af184e7ded6f885f17c69abe3ea7c429c30fd600a999af4c9212b6336cbb1a2eb138bb294dfc1bf253bd0aa0a93  dnsmasq-regex-2.78.zip
b07055d71e535f753aff432124812fbef86cc2f490ff2a4704959c34b0f69caa74791a4ad08b2b8638c9126233591d3a86c188965eb1308e7e7c12dc0039d1ad  dnsmasq.initd
9a401bfc408bf1638645c61b8ca734bea0a09ef79fb36648ec7ef21666257234254bbe6c73c82cc23aa1779ddcdda0e6baa2c041866f16dfb9c4e0ba9133eab8  dnsmasq.confd
d01077f39e1240041a6700137810f254daf683b2d58dafecb6b162e94d694992e57d45964a57993b298f97c2b589eedcf9fb1506692730a38b7f06b5f55ba8d8  uncomment-conf-dir.patch"

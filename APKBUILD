# Maintainer: Cameron Banta <cbanta@gmail.com>
# Contributor: Jeff Bilyk <jbilyk@gmail.com>
# Contributor: Bartłomiej Piotrowski <nospam@bpiotrowski.pl>
# Contributor: Andy Shinn <andys@andyshinn.as>
# Contributor: Matt Diebolt <mdiebolt@gmail.com>

pkgname=nginx-extra-geoip
_pkgname=nginx
pkgver=1.6.2
_nginxrtmpver=1.1.6
pkgrel=1
pkgdesc="lightweight HTTP and reverse proxy server"
url="http://www.nginx.org"
arch="all"
license="Custom"
install="$pkgname.pre-install $pkgname.pre-upgrade"
makedepends="pcre-dev openssl-dev zlib-dev"
subpackages="$_pkgname-doc $_pkgname-vim:vim"
source="http://nginx.org/download/$_pkgname-$pkgver.tar.gz
	nginx-rtmp-module-$_nginxrtmpver.tar.gz::https://github.com/arut/nginx-rtmp-module/archive/v$_nginxrtmpver.tar.gz
	musl-crypt-fix.patch
	ipv6.patch

	nginx.initd
	nginx.logrotate
	"

_builddir="$srcdir"/$_pkgname-$pkgver

prepare() {
	cd "$_builddir"
	for i in $source; do
		case $i in
		*.patch) msg $i; patch -p1 -i "$srcdir"/$i || return 1;;
		esac
	done
}


build() {
	cd "$_builddir"
	./configure \
		--prefix=/usr \
		--conf-path=/etc/$_pkgname/$_pkgname.conf \
		--pid-path=/var/run/$_pkgname.pid \
		--lock-path=/var/run/$_pkgname.lock \
		--error-log-path=/var/log/$_pkgname/error.log \
		--http-log-path=/var/log/$_pkgname/access.log \
		--http-client-body-temp-path=/tmp/$_pkgname/client-body \
		--http-proxy-temp-path=/tmp/$_pkgname/proxy \
		--http-fastcgi-temp-path=/tmp/$_pkgname/fastcgi \
		--user=nginx \
		--group=nginx \
		--with-ipv6 \
		--with-pcre-jit \
		--with-http_dav_module \
		--with-http_ssl_module \
		--with-http_stub_status_module \
		--with-http_geoip_module \
		--with-http_gzip_static_module \
		--with-http_spdy_module \
		--with-mail \
		--with-mail_ssl_module \
		--add-module="$srcdir/nginx-rtmp-module-$_nginxrtmpver" \
		|| return 1
	make || return 1
}

package() {
	cd "$_builddir"
	make DESTDIR="$pkgdir" install

	install -m755 -D "$srcdir"/$_pkgname.initd "$pkgdir"/etc/init.d/$_pkgname
	install -m644 -D "$srcdir"/$_pkgname.logrotate "$pkgdir"/etc/logrotate.d/$_pkgname

	install -m644 -D LICENSE "$pkgdir"/usr/share/licenses/$_pkgname/LICENSE
	install -m644 -D man/$_pkgname.8 "$pkgdir"/usr/share/man/man8/$_pkgname.8
}

vim() {
	local t

	depends=""
	pkgdesc="Vim syntax for Nginx"
	arch="noarch"

	for t in ftdetect syntax indent; do
		install -Dm644 "$_builddir"/contrib/vim/$t/$_pkgname.vim \
			"$subpkgdir"/usr/share/vim/vimfiles/$t/$_pkgname.vim
	done
}

md5sums="d1b55031ae6e4bce37f8776b94d8b930  nginx-1.6.2.tar.gz
ceca9874cd89e6027fae8178f5c3af86  nginx-rtmp-module-1.1.6.tar.gz
3aeb488921109e60d02ed64d36790aeb  musl-crypt-fix.patch
801a87f7f9d27f8ad85b41a78b4c4461  ipv6.patch
9f5db5e24ce9f671978eb3b1b5e6e817  nginx.initd
d3f30c25c84c55252a6babd9e9b0325b  nginx.logrotate"
sha256sums="b5608c2959d3e7ad09b20fc8f9e5bd4bc87b3bc8ba5936a513c04ed8f1391a18  nginx-1.6.2.tar.gz
4039d1e7febd93188f729b594772d04d8a1137b2e90b12fa53bb061f200add87  nginx-rtmp-module-1.1.6.tar.gz
8c398640bd379c1c6a2fafcd2b3848a72902e47924e8e2490b312c141eec5d70  musl-crypt-fix.patch
a24ef5843ae0afa538b00c37eb7da7870f9d7f146f52a9668678f7296cf71d9b  ipv6.patch
4feea52915e03ab14ad985254b81a762ee29b9553993b63dd49bedb24a3ae416  nginx.initd
6b89872994508cc7b4b225bca3301d7942767f37b8b691134141d95995740890  nginx.logrotate"
sha512sums="5698655ebe847bab7cc2531711f02e17257e361559ecf13d424019a5f8f07f54e78c4c73f97e5de063c4a70d13a236ca855b73a962053cd21d44f9621a5ac600  nginx-1.6.2.tar.gz
6db0cc5a3cff600a836483f9cc4ff76860e9c893167561ad818cb41e2eb4fa31af8a4213e42c7c5766e389aed0ad713cffe776aa4bc4ebf279dd63eb65d4162c  nginx-rtmp-module-1.1.6.tar.gz
21114c775e4bdd1f7b8b9abc143284945e96ed1d8c49904ddf918abad87b16253f918ba47976cd2df32f0fdb8a7dad399d4200e879db2da6cf93a28aab236a75  musl-crypt-fix.patch
68d64a84568ec2df0366925ab282a05ebe21a85044b6c7844a47573cfd8cc8ed119cc772358bc3fff36e2d4fdf583a730592825f5f98632993ca86d1f8438d5f  ipv6.patch
3f302d782f81cb70634bcdf079c842b0783d8d492c3d2a9bb8e54aae0392818ee8ab2d85d2b718a3563c48edeb349f8443487d631310fcca7b70f91967417d47  nginx.initd
fda91710185d6b801dd746c8c3678b5719b408de0b715bef7b1985f1ee17db1e8378d440759ea6234b1f70454a35870a2917bd1d6cd309ddc70e1c066fc8d4b8  nginx.logrotate"

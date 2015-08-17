# Maintainer: Daniel Wallace <daniel.wallace at gatech dot edu>
# Contributor: Cyker Way <cykerway at gmail dot com>
pkgname=pianobarfly-patched-git
_pkgname=pianobarfly
pkgver=20120514
pkgrel=1
pkgdesc="A console client for the personalized web radio pandora, patched for the overwrite errors"
url="http://www.ghuntley.com/"
arch=('i686' 'x86_64')
license=('MIT')
depends=('libao' 'faad2' 'libmad' 'readline')
optdepends=('libmad')
makedepends=('pkgconfig>=0.9' 'git' 'automake')
provides=('pianobarfly')
conflicts=('pianobarfly-git')
replaces=('pianobarfly-git')
source=(https://raw.github.com/gist/2500616/bf2fc7c339791be9dce8ebf0ecca3f6647c9e3f2/gistfile1
        pianobarfly.patch)

_gitroot="https://github.com/nega0/$_pkgname.git"
_gitname="$_pkgname"
_gitbranch="mkstemp"

build() {
	cd $srcdir
	msg "Connecting to the pianobarfly git repository..."

	if [ -d "$srcdir/$_gitname" ] ; then
		cd $_gitname && git pull origin
		msg "The local files are updated."
	else
		git clone -b $_gitbranch $_gitroot $_gitname
	fi

	msg "GIT checkout done or server timeout"
	msg "Starting make..."

	rm -rf "$srcdir/$_gitname-build"
	git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
	cd "$srcdir/$_gitname-build/"
	LIBS="-lm"
   # patch -Np1 -i $srcdir/pianobarfly.patch
   sed -i 's:${LIBFAAD_LDFLAGS} ${LIBMAD_LDFLAGS} ${LIBGNUTLS_LDFLAGS} ${LIBID3TAG_LDFLAGS} -o $@:& /lib/libm.so.6:' Makefile
	make
}
package() {
	cd "$srcdir/$_gitname-build"
	make DESTDIR=$pkgdir PREFIX=/usr install

	install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
	install -m644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/"
}
md5sums=('5805e5f662733a0ee6c395270483b526'
         'dc816221374f2c8a1f33850201647530')

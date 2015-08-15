# Maintainer: Ivan de Jesús Pompa García <ivan.pompa@gmx.com>
# Contributors: niQo ???

pkgname=bamf2
_dname=bamf
pkgver=0.2.126
pkgrel=2
pkgdesc="Removes the headache of applications matching into a simple DBus daemon and c wrapper library, 0.2 branch"
arch=('i686' 'x86_64')
url="https://launchpad.net/bamf"
license=('GPL')
depends=('dbus-glib' 'libwnck3' 'libgtop')
makedepends=('libwnck' 'vala')
optdepends=('gtk2: GTK+ 2 library')
options=(!libtool)
source=(http://launchpad.net/${_dname}/0.2/${pkgver}/+download/${_dname}-${pkgver}.tar.gz)
md5sums=('709735137e4b028bb94f9e106bb9ac6e')

conflicts=('bamf')
provides=('bamf')

build() {
  cd "$srcdir/${_dname}-${pkgver}"

  # Disable building tests
  sed -i '/tests/ d' Makefile.in

  sed -i -e 's/--c-include/--include/' lib/libbamf/Makefile.in
  export CFLAGS="$CFLAGS -Wno-deprecated-declarations -Wno-unused-local-typedefs"

  [[ -d build-gtk3 ]] || mkdir build-gtk3
  pushd build-gtk3
  ../configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --libexecdir=/usr/lib/${_dname} \
              --disable-static
  make
  popd

  [[ -d build-gtk2 ]] || mkdir build-gtk2
  pushd build-gtk2
  ../configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --libexecdir=/usr/lib/${_dname} \
              --disable-static --with-gtk=2
  make -C lib/libbamf
  popd
}

package() {
  cd "$srcdir/${_dname}-${pkgver}/build-gtk3"
  make DESTDIR="${pkgdir}/" install

  cd "$srcdir/${_dname}-${pkgver}/build-gtk2"
  make -C lib/libbamf DESTDIR="${pkgdir}" install
}

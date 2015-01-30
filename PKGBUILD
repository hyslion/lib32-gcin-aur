# Maintainer: Yushin Huang <hyslion AT gmail.com>

pkgbase=lib32-gcin
pkgname=lib32-gcin
_pkgname=gcin
pkgver=2.8.3
pkgrel=1
true && pkgname=('lib32-gcin-im-client' 'lib32-gcin-qt4' 'lib32-gcin-gtk2')
url='http://hyperrate.com/dir.php?eid=67'
license=('LGPL')
arch=('x86_64')
depends=("gcin=${pkgver}")
makedepends=('lib32-gtk2' 'lib32-qt4' 'gcc-multilib')
source=("http://hyperrate.com/gcin-source/${_pkgname}-${pkgver}.tar.xz"
        "qconfig.h"
        "lib32-gcin.patch")
sha1sums=('95b980f7cf9dc2ad91803113cdbb4ac831f056de'
          '85dba460dc77e0cd07d07a33388a345cc91c98d3'
          '4f64364d34aa48e50ce142ad556c6521d7f0b42d')

prepare() {
  # add qconfig.h for 32bit Qt im-module
  mkdir -p "${srcdir}/QtCore"
  cp qconfig.h "${srcdir}/QtCore/"

  export CC="gcc -m32 -I${srcdir} -I${srcdir}/QtCore"
  export CXX="g++ -m32 -I${srcdir} -I${srcdir}/QtCore"

  # use qt4 because the /usr/bin/moc is a symlink to 'qtchooser'(qt5) now
  export QT_SELECT=4

  cd "${srcdir}/${_pkgname}-${pkgver}"

  patch -p1 < ${srcdir}/lib32-gcin.patch

  sed '/include suffixes-rule/a \
    CFLAGS+='"${CFLAGS}"' \
    LDFLAGS+='"${LDFLAGS}"' \
    OPTFLAGS=' \
    -i Makefile

  ./configure           \
    --prefix=/usr     \
    --use_i18n=Y
}

build() {
  cd "${srcdir}/${_pkgname}-${pkgver}"

  # due to dependency, gtk-im will be built together
  make -C im-client

  # build Qt4 im-module
  make -C qt4-im
}

package_lib32-gcin-im-client() {
  pkgdesc='gcin 32-bit im-client module'
  depends=("gcin=${pkgver}")

  cd "${srcdir}/${_pkgname}-${pkgver}/im-client"
  make DESTDIR="${pkgdir}" install
}

package_lib32-gcin-gtk2() {
  pkgdesc='gcin im-module for 32-bit GTK-based applications'
  depends=("lib32-gcin-im-client=${pkgver}")
  install=lib32-gcin-gtk2.install

  cd "${srcdir}/${_pkgname}-${pkgver}/gtk-im"
  make DESTDIR="${pkgdir}" install
}

package_lib32-gcin-qt4() {
  pkgdesc='gcin im-module for 32-bit Qt4-based applications'
  depends=("lib32-gcin-im-client=${pkgver}")

  cd "${srcdir}/${_pkgname}-${pkgver}/qt4-im"
  make DESTDIR="${pkgdir}" install
}

pkgdesc='gcin im-module for 32-bit Gtk, Qt4 applications'

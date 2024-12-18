# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Edmund Lodewijks <e.lodewijks at gmail. com>
# Contributor: Christian Hesse <mail@eworm.de>

_proj="gnome"
_pkg=gtkglext
pkgname="${_pkg}"
pkgver=1.2.0
_pkgvershort='1.2'
pkgrel=20
pkgdesc='An OpenGL extension to GTK2.'
arch=(
  'x86_64'
  'i686'
  'arm'
  'aarch64'
  'armv7l'
  'armv6l'
  'mips'
  'pentium4'
  'powerpc'
)
_sf="https://sourceforge.net"
url_sf="${_sf}/projects/${_pkg}"
_gnome="https://gitlab.${_proj}.org"
_http="${_gnome}"
_ns="Archive"
url="${_http}/${_ns}/${_pkg}"
license=(
  'LGPL-2.0-or-later'
)
depends=(
  'glib2'
  'pango'
  'gtk2'
  'glu'
  'libxmu'
  'libgl'
)
makedepends=(
  'glib2-devel'
)
_gnome_sources="https://download.${_proj}.org/sources"
_url="${_gnome_sources}/${_pkg}"
_patches=(
  '001-no-pangox.patch'
  '002-fix-gtk2.20-deprecated-symbols.patch'
  "003-${_pkg}-gcc8.patch"
  '004-fix-gdk-gdkglshapes.c.patch'
)
source=(
  "${_url}/${_pkgvershort}/${_pkg}-${pkgver}.tar.bz2"
  # The SourceForge source is used by some distro's. Others use gnome.
  # "${url_sf}/files/${_pkg}/${pkgver}/${_pkg}-${pkgver}.tar.bz2"
  "${_patches[@]}"
)
sha256sums=(
  '16bd736074f6b14180f206b7e91263fc721b49912ea3258ab5f094cfa5497f51'
  '8ce31aa17ea84aede3b03fa392ec95e0e9c001f348384ba93d850de9f0b7044e'
  '864c6963c4a2f4f69f1f028ecee6c821a4f4d5aba142f2e07898aede825ce9ea'
  '0ec0c22b15054b0684f9a9015ff05920af0c00dae5409e76321f683dcc17cff9'
  '499c49347df977f1d801eed5df0fd6398c1b1493c8a36d6d61bb6a7775eb83db'
)

prepare() {
  local \
    _patch
  cd \
    "${_pkg}-${pkgver}"
  for _patch in "${_patches[@]}"; do
    patch \
      -Np1 < \
      "${srcdir}/${_patch}"
  done
  mv \
    "configure."{"in","ac"}
  sed \
    '/AC_PATH_XTRA/d' \
    -i "configure.ac"
  autoreconf \
    -vi
}
  
build() {
  cd \
    "${_pkg}-${pkgver}"
  ./configure \
    --prefix=/usr \
    --disable-static
  sed \
    -i \
    -e \
      's/ -shared / -Wl,-O1,--as-needed\0/g' \
    "libtool"
  make
}

package() {
  cd \
    "${_pkg}-${pkgver}"
  make \
    DESTDIR="${pkgdir}" \
    install
}

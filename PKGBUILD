# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Yunhui Fu <yhfudev@gmail.com>

_pkg="xavs"
pkgname="${_pkg}"
pkgver=0.1.55
pkgrel=1
_pkgdesc=(
  "XAVS is to implement high quality"
  "encoder and decoder of the"
  "Audio Video Standard of China (AVS)."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'i686'
  'x86_64'
  'arm'
  'armv7l'
  'armv6l'
  'aarch64'
  'mips'
  'powerpc'
  'pentium4'
)
url="http://${_pkg}.sourceforge.net/"
license=(
  'GPL'
)
depends=()
makedepends=(
  'yasm'
)
# options=(
#   !strip
# )
_http="https://github.com"
_ns="OpenMandrivaAssociation"
_url="${_http}/${_ns}/${_pkg}"
_patches=(
  "${_pkg}-0.1.55-dont-strip-symbols.patch"
  "${_pkg}/raw/master/${_pkg}-dynamic-${_pkg}.patch"
  "${_pkg}-dup-asm.patch"
  "${_pkg}-x32-yasm.patch"
)
source=(
  # pkgname=xavs, pkgver=0.1.55
  "${pkgname}-${pkgver}.tar.gz::${_url}/raw/master/${_pkg}-${pkgver}.tar.xz"
  "${_url}/raw/master/${_pkg}-0.1.55-dont-strip-symbols.patch"
  "${_http}/pld-linux/${_pkg}/raw/master/${_pkg}-dynamic-${_pkg}.patch"
  "${_pkg}-dup-asm.patch"
  "${_pkg}-x32-yasm.patch"
)
md5sums=(
  '2986b9829e016e9800e50278469fdfae'
  '8ce1d21e378d234b949cb035c66d5655'
  'f6c2726fc2c11025b868952f389c0db3'
  'b39717e48edb5e8b8696e50d41c24ac0'
  'd42b193cb6f8c9bfd62a0631698e29f0'
)

prepare() {
  local \
    _patch
  cd \
    "${srcdir}/${pkgname}-${pkgver}"
  for _patch in "${_patch[@]}"; do
    patch \
      -Np1 \
      -i \
      "${srcdir}/${_patch}"
  done
  sed \
    -i \
    -e \
      's|$(CC) -o $@ $(OBJCLI) $(LDFLAGS) -L. -lxavs|$(CC) -o $@ $(OBJCLI) -L. -lxavs $(LDFLAGS)|' \
    Makefile
  sed \
    -i \
    -e \
      's|xavs$(EXE): $(OBJCLI) $(SONAME)|xavs$(EXE): $(OBJCLI) $(SONAME) libxavs.a|' \
    Makefile
}

build() {
  cd \
    "${srcdir}/${pkgname}-${pkgver}"
  ./configure \
    --enable-shared \
    --disable-asm \
    --prefix="/usr"
  make
}

package() {
  cd \
    "$srcdir/${pkgname}-${pkgver}"
  make \
    DESTDIR="${pkgdir}" \
    install
  install \
    -Dm644 \
    "lib${_pkg}.a" \
    "${pkgdir}/usr/lib/lib${_pkg}.a"
}

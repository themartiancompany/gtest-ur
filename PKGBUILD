# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer:
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer:
#   Carl Smedstad
#     <carsme@archlinux.org>
# Maintainer:
#   Levente Polyak
#     <anthraxx[at]archlinux[dot]org>
# Contributor:
#   Felix Yan
#     <felixonmars@archlinux.org>
# Contributor:
#   Jan Alexander Steffens (heftig)
#     <jan.steffens@gmail.com>
# Contributor:
#   Alexandre Bique
#     <bique.alexandre@gmail.com>
# Contributor:
#   Louis R. Marascio
#     <lrm@fitnr.com>
# Contributor:
#   Cody Maloney
#     <cmaloney@theoreticalchaos.com>
# Contributor:
#   acxz
#     <akashpatel2008 at yahoo dot com>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
  _libcompiler="libllvm"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
  _libcompiler="gcc-libs"
fi
_py="python"
_pkg=gtest
_Pkg="googletest"
pkgname="${_pkg}"
pkgver=1.17.0
pkgrel=1
_pkgdesc=(
  'Google Test - C++ testing utility'
)
pkgdesc="${_pkgdesc[*]}"
_http="https://github.com"
_ns="google"
url="${_http}/${_ns}/${_Pkg}"
arch=(
  'arm'
  'aarch64'
  'i686'
  'x86_64'
)
license=(
  'BSD-3-Clause'
)
depends=(
  "${_libcompiler}"
  "${_libc}"
)
makedepends=(
  'cmake'
  "${_py}"
)
_py_optdepends=(
  "${_py}:"
    "gmock generator."
)
optdepends=(
  "${_py_optdepends[*]}"
)
conflicts=(
  'gmock'
)
replaces=(
  'gmock'
)
provides=(
  'gmock'
  'libgmock.so'
  'libgmock_main.so'
  "lib${_pkg}.so"
  "lib${_pkg}_main.so"
)
_tarname="${_Pkg}-${pkgver}"
_src="${_tarname}.tar.gz::${url}/archive/v${pkgver}.tar.gz"
_sum='0f57e9ef06925e5b7722df1eb92ef5850e8dce79220ea16a8aaff586a71c0b01460ef1713649ee24ffedb2e6ad5a51e9198c5a5ae1b2789e43feb1f494e7d45c'
source=(
  "${_src}"
)
sha512sums=(
  "${_sum}"
)

build() {
  local \
    _cmake_opts=()
  _cmake_opts+=(
   -H"${_tarname}"
   -B"build"
   -DCMAKE_INSTALL_PREFIX="/usr"
   -DCMAKE_BUILD_TYPE="None"
   -Wno-dev
   -DBUILD_SHARED_LIBS="ON"
   -Dgtest_build_tests="ON"
   -DGOOGLETEST_VERSION="${pkgver}"
  )
  cmake \
    "${_cmake_opts[@]}"
  cmake \
    --build \
      "build"
}

check() {
  cmake \
    --build \
      "build" \
    --target \
      "test"
}

package() {
  DESTDIR="${pkgdir}" \
  cmake \
    --install \
      "build"
  cd \
    ${_tarname}
  install \
    -vDm644 \
    "LICENSE" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  install \
    -vDm644 \
    "README.md" \
    "CONTRIBUTORS" \
    -t \
    "${pkgdir}/usr/share/doc/${pkgname}"
}

# vim: ts=2 sw=2 et:

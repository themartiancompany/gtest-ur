# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025, 2026  Pellegrino Prevete
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

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
#   Carl Smedstad
#     <carsme@archlinux.org>
#   Levente Polyak
#     <anthraxx[at]archlinux[dot]org>
# Contributors:
#   Felix Yan
#     <felixonmars@archlinux.org>
#   Jan Alexander Steffens (heftig)
#     <jan.steffens@gmail.com>
#   Alexandre Bique
#     <bique.alexandre@gmail.com>
#   Louis R. Marascio
#     <lrm@fitnr.com>
#   Cody Maloney
#     <cmaloney@theoreticalchaos.com>
#   acxz
#     <akashpatel2008 at yahoo dot com>

_os="$(
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
  _libcompiler="libc++"
  _compiler="clang"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _compiler="gcc"
  _libc="glibc"
  _libcompiler="gcc-libs"
elif [[ "${_os}" == "Msys" ]]; then
  _libc="msys2-w32api-runtime"
  _libc_headers="msys2-w32api-headers"
  _compiler="gcc"
  _libcompiler="gcc-libs"
  _sh="sh"
  _mailcap="winpty"
fi
if [[ ! -v "_tests" ]]; then
  _tests="false"
fi
_py="python"
_pkg=gtest
_Pkg="googletest"
pkgbase="${_pkg}"
pkgname=(
  "${_pkg}"
)
pkgver=1.18.0
pkgrel=4
_pkgdesc=(
  'Google Test - C++ testing utility'
)
pkgdesc="${_pkgdesc[*]}"
_http="https://github.com"
_ns="google"
url="${_http}/${_ns}/${_Pkg}"
arch=(
  'aarch64'
  'arm'
  'armv7l'
  'armv8l'
  'i686'
  'mips'
  'pentium4'
  'powerpc'
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
  "${_compiler}"
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
  "googletest"
  'libgmock.so'
  'libgmock_main.so'
  "lib${_pkg}.so"
  "lib${_pkg}_main.so"
)
_tarname="${_Pkg}-${pkgver}"
_src="${_tarname}.tar.gz::${url}/archive/v${pkgver}.tar.gz"
_sum="6e3191c1455468b3fc35a417fb565c1c5071aee1b7e7f85e30cf48a98d37d8b5"
source=(
  "${_src}"
)
sha256sums=(
  "${_sum}"
)

prepare() {
  local \
    _version
  _version="$(
    sed \
      -En \
        's/^set\(GOOGLETEST_VERSION\s+([0-9.]+).*/\1/p' \
      "${_tarname}/CMakeLists.txt")"
  if [[ "${pkgver}" != "${_version}"  ]]; then
    _msg=(
      "Version detected from sources"
      "different from version declared"
      "in the package."
    )
    echo \
      "${_msg[*]}" \
      1>&2
    exit \
      1
  fi
}

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
   -DGOOGLETEST_VERSION="${pkgver}"
  )
  if [[ "${_tests}" == "true" ]]; then
    _cmake_opts+=(
      -Dgtest_build_tests="ON"
    )
  fi
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

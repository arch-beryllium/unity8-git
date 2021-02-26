pkgbase=unity8
pkgname=(unity8-git indicators-client-git)
_pkgname=unity8
pkgver=r18273.78cac2dcc
pkgrel=2
url='https://github.com/ubports/unity8'
arch=(x86_64 i686 armv7h aarch64)
license=(LGPL3)
depends=(lcov gcovr geonames unity-api qmenumodel gnome-desktop ubuntu-app-launch ubuntu-ui-toolkit libqtdbustest libqtdbusmock system-settings indicator-network libusermetrics liblightdm-qt5 settings-components gsettings-ubuntu-touch-schemas deviceinfo qtmir telephony-service unity-notifications biometryd thumbnailer hfd-service)
makedepends=(git cmake cmake-extras doxygen)
checkdepends=(xorg-server-xvfb)
source=('git+https://github.com/ubports/unity8.git#branch=xenial_-_edge_-_wayland_-_x11'
        'https://gitlab.manjaro.org/manjaro-arm/packages/community/lomiri-dev/unity8-git/-/raw/master/NoDpkgParse.patch'
        'alarmify-lomiri.patch'
        'https://gitlab.manjaro.org/manjaro-arm/packages/community/lomiri-dev/unity8-git/-/raw/master/skip-language-wizard-page.patch'
        'https://gitlab.manjaro.org/manjaro-arm/packages/community/lomiri-dev/unity8-git/-/raw/master/0001-Use-deviceinfo-for-platform.patch'
        'https://gitlab.manjaro.org/manjaro-arm/packages/community/lomiri-dev/unity8-git/-/raw/master/0001-Move-indicator-dbus-calls-to-org.ayatana.patch')
sha256sums=('SKIP'
            'e5d335f3a508aa2660e0c04719dd79b391f3badaff1e6072409fe60c30847482'
            'cd9fc796063460e54cf879382f97991e081b7147fe30bb8e57493e9104e6925c'
            'f28deb38ccc6556a749d6d4f0bccd7e6a0166c2880d3fcb78898343c274f92f3'
            '273f7b342642ed5240cd138911395ee72f45d3cb1509e3f6b78b3f5ec9d5c233'
            '905c7e4798a8cc41649d532ee097a63f98c85507ec29059f353a082ad0b0f0a0')

BUILDENV+=('!check') # https://paste.ubuntu.com/p/2d33RJX4W8/

pkgver() {
  cd ${_pkgname}
  echo "r$(git rev-list --count HEAD).$(git describe --always)"
}

prepare() {
  cd ${_pkgname}
  sed -i 's/ubuntu-app-launch-2/ubuntu-app-launch-3/g' CMakeLists.txt
  patch -Np1 -i "${srcdir}/alarmify-lomiri.patch"
  patch -Np1 -i "${srcdir}/NoDpkgParse.patch"
  patch -Np1 -i "${srcdir}/skip-language-wizard-page.patch"
  patch -Np1 -i "${srcdir}/0001-Use-deviceinfo-for-platform.patch"
  patch -Np1 -i "${srcdir}/0001-Move-indicator-dbus-calls-to-org.ayatana.patch"
}

build() {
  cd ${_pkgname}
  mkdir -p build
  cd build
  cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_INSTALL_LIBDIR="lib/" -DNO_TESTS=true ..
  make
}

package_unity8-git() {
  pkgdesc='Unity 8 shell'
  conflicts=(unity8)
  provides=(unity8)
  cd ${_pkgname}/build
  make DESTDIR="${pkgdir}/" install

  # Cleanup
  rm -v ${pkgdir}/usr/bin/indicators-client
  rm -v ${pkgdir}/usr/share/applications/indicators-client.desktop

  # Move conf to the right directory
   #mv "${pkgdir}/usr/etc/dbus-1/system.d/com.canonical.Unity.conf"  "${pkgdir}/etc/dbus-1/system.d/"
}

package_indicators-client-git() {
  pkgdesc='Unity 8 indicators client'
  conflicts=(indicators-client)
  provides=(indicators-client)
  cd ${_pkgname}/build
  make DESTDIR="${pkgdir}/" install

  # Cleanup
  rm -vr ${pkgdir}/usr/{lib,var}
  rm -vr ${pkgdir}/usr/share/{accountsservice,dbus-1,glib-2.0,lightdm,locale,unity8}
  rm -v ${pkgdir}/usr/share/applications/unity8.desktop
  rm -v ${pkgdir}/usr/bin/unity8
}

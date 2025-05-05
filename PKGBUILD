pkgname=AmneziaVPN
pkgver=4.8.5.0
pkgrel=1
pkgdesc="Amnezia VPN Client"
arch=('x86_64')
url="https://github.com/amnezia-vpn/amnezia-client"
license=('custom')
makedepends=('cmake' 'vulkan-headers' 'qt6-base' 'qt6-remoteobjects' 'qt6-5compat' 'qt6-tools' 'clang' '7zip')
depends=()
source=("git+https://github.com/amnezia-vpn/amnezia-client#tag=${pkgver}")
sha256sums=('SKIP') # Replace 'SKIP' with the actual checksum for better security

prepare() {
    cd "${srcdir}/amnezia-client"
    git submodule update --init --recursive

    # add missing "#include <QMap>""
    sed -i 's/#include "ipaddress.h"/#include "ipaddress.h"\n#include <QMap>/' client/daemon/interfaceconfig.h
}

package() {
    cd "${srcdir}/amnezia-client"
    QT_BIN_DIR=/lib/qt6/bin ./deploy/build_linux.sh

    # # Create the installation directory
    install -Dm755 -d "${pkgdir}/opt/${pkgname}"

    # Copy the contents of the extracted directory to the installation directory
    # install -D -d "${srcdir}/amnezia-client/deploy/AppDir" "${pkgdir}/opt/${pkgname}"
    cp -r "${srcdir}/amnezia-client/deploy/AppDir/." "${pkgdir}/opt/${pkgname}/"

    # Create a symlink to the executable in /usr/bin
    mkdir -p "${pkgdir}/usr/bin"
    ln -s "../../opt/${pkgname}/client/AmneziaVPN.sh" "${pkgdir}/usr/bin/AmneziaVPN"

    # Install background service
    mkdir -p "${pkgdir}/etc/systemd/system"
    ln -s "../../../opt/${pkgname}/AmneziaVPN.service" "${pkgdir}/etc/systemd/system/AmneziaVPN.service"

    # Install application launcher
    install -Dm644 "${pkgdir}/opt/${pkgname}/AmneziaVPN.desktop" "${pkgdir}/usr/share/applications/AmneziaVPN.desktop"
    install -Dm644 "${pkgdir}/opt/${pkgname}/AmneziaVPN.png" "${pkgdir}/usr/share/pixmaps/AmneziaVPN.png"

    # Install license if available
    install -Dm644 "${srcdir}/amnezia-client/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

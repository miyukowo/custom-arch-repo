# Maintainer: miyukowo <https://github.com/miyukowo>

pkgname=antigravity
pkgver=2.23.1
pkgrel=1
_build=6348059836284928
pkgdesc='Google Antigravity 2.0 multi-agent orchestration platform'
arch=('x86_64')
url='https://antigravity.google/'
license=('LicenseRef-Google-Antigravity')
depends=(
    'alsa-lib'
    'at-spi2-core'
    'cairo'
    'dbus'
    'expat'
    'glib2'
    'glibc'
    'gtk3'
    'libcups'
    'libgcc'
    'libx11'
    'libxcb'
    'libxcomposite'
    'libxdamage'
    'libxext'
    'libxfixes'
    'libxkbcommon'
    'libxrandr'
    'mesa'
    'nspr'
    'nss'
    'pango'
    'systemd-libs'
)
provides=('antigravity' 'antigravity-appimage')
conflicts=('antigravity-appimage')
replaces=('antigravity-appimage')
options=('!strip' '!debug')
source=(
    'antigravity.desktop'
    'antigravity.png'
    "Antigravity-${pkgver}-x86_64.tar.gz::https://storage.googleapis.com/antigravity-public/antigravity-hub/${pkgver}-${_build}/linux-x64/Antigravity.tar.gz"
)
b2sums=('SKIP'
        'SKIP'
        'SKIP')

package() {
    install -d "${pkgdir}/opt"
    cp -a Antigravity-*/ "${pkgdir}/opt/Antigravity"

    install -d "${pkgdir}/usr/bin"
    ln -s /opt/Antigravity/antigravity "${pkgdir}/usr/bin/${pkgname}"

    install -d "${pkgdir}/usr/share/licenses/${pkgname}"
    ln -s /opt/Antigravity/LICENSE.electron.txt -t "${pkgdir}/usr/share/licenses/${pkgname}" 2>/dev/null || true
    ln -s /opt/Antigravity/LICENSES.chromium.html -t "${pkgdir}/usr/share/licenses/${pkgname}" 2>/dev/null || true

    install -Dm644 antigravity.desktop -t "${pkgdir}/usr/share/applications"
    install -Dm644 antigravity.png -t "${pkgdir}/usr/share/pixmaps"
}

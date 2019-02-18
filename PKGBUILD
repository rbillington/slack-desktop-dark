# Maintainer: Seamus Connor
# Contributor: Ryan Billington

pkgname=slack-desktop-dark
pkgver=3.3.7
pkgrel=7
pkgdesc="Slack Desktop (Beta) for Linux, with dark theme patch"
arch=('x86_64')
url="https://slack.com/downloads"
license=('custom')
depends=('alsa-lib' 'gconf' 'gtk3' 'libcurl-compat' 'libsecret' 'libxss' 'libxtst' 'nss' 'glibc>=2.28-4')
optdepends=('gnome-keyring')
conflicts=('slack-desktop')
source=("https://downloads.slack-edge.com/linux_releases/${pkgname%-dark}-${pkgver}-amd64.deb"
		"darkify_slack_preamble.js"
		"darkify_slack.css"
		"darkify_slack_custom.css"
		"darkify_slack_afterword.js"
    	"${pkgname}.patch")
noextract=("${pkgname%-dark}-${pkgver}-amd64.deb")
sha256sums=('17310bc323eafcef86c134c7aea9b53a82f8394aa30a886ac419f9a5a23168e0'
            'ffbe44c64f8a25816ea78731f97af49e1aab530e942e432f3836a0a9a1ab62a5'
            '688842a1421c28b374b6d2a0ba83e1ba2d7c8bcaa3104e2810bf03693e70a434'
            'be9858241b233acd05e5f1c08f6e1249d5784da2befb1981b68c456b9f40538c'
            '4f1a6104256ccdb7ceb85a4272477e87cca2aaaeef4cb0cab1d2ad03554e815d'
            'c952eb32dd59beff9fc5374853b04acde4a60ed8c39934fcd0b66829455d594d')

package() {
    bsdtar -O -xf "slack-desktop-${pkgver}"*.deb data.tar.xz | bsdtar -C "${pkgdir}" -xJf -

    # Fix hardcoded icon path in .desktop file
    patch -d "${pkgdir}" -p1 <"${pkgname}".patch

    # Permission fix
    find "${pkgdir}" -type d -exec chmod 755 {} +

    # Remove all unnecessary stuff
    rm -rf "${pkgdir}/etc"
    rm -rf "${pkgdir}/usr/share/lintian"
    rm -rf "${pkgdir}/usr/share/doc"

    cat darkify_slack_preamble.js darkify_slack.css darkify_slack_custom.css darkify_slack_afterword.js >> "${pkgdir}/usr/lib/slack/resources/app.asar.unpacked/src/static/ssb-interop.js"

    # Move license
    install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"
    mv "${pkgdir}/usr/lib/slack/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}"
    ln -s "/usr/share/licenses/${pkgname}/LICENSE" "${pkgdir}/usr/lib/slack/LICENSE"
}

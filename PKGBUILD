## Maintainer:     barfin <barfin@protonmail.com>
## Co-Maintainer:  Jaja <jaja@mailbox.org>

## pkginfo
pkgdesc="A fancy custom distribution of Valves Proton with various patches"
pkgname=proton-ge-custom-bin
pkgver=5.2_GE_2
pkgrel=2
arch=('x86_64')
license=('BSD' 'LGPL' 'zlib' 'MIT' 'MPL' 'custom')
provides=('proton')
depends=('python')
optdepends=('winetricks: protonfixes backend'
            'wine-staging: 32bit prefixes'
            'python-cef: splash dialog support'
            'zenity: splash dialog support'
            'steam: use proton with steam like intended')

## fix naming conventions, matching upstream
_pkgname=${pkgname//-bin/}
_pkgver=${pkgver//_/-}
_srcdir=Proton-${_pkgver}

## paths and files
_protondir=usr/share/steam/compatibilitytools.d/${_pkgver}
_licensedir=usr/share/licenses/${_pkgname}
_pfxdir=var/games/pfx_${_pkgver}
_execfile=usr/local/bin/proton
#_configfile=usr/local/etc/proton/user_settings.py
#backup=("${_configfile}")

## sources
url='https://github.com/GloriousEggroll/proton-ge-custom'
source=(${_pkgname}-${_pkgver}.tar.gz::"${url}/releases/download/${_pkgver}/${_srcdir}.tar.gz"
        "supplementary.tar.zst")
md5sums=('457c948807ecd47c4c4e692485513064'
         'de6e765583be1d89345a2b9c796ccbb0')

prepare() {
## unpack wine
install -d ${_srcdir}/dist
bsdtar -xf ${_srcdir}/proton_dist.tar.gz -C ${_srcdir}/dist/
rm ${_srcdir}/proton_dist.tar.gz
}

build() {
## remove unused: dist_lock, extract_tarball(), make_default_prefix()
patch ${_srcdir}/proton patches/distlock-extract-defaultpfx.patch >/dev/null
## setup paths
#sed -i "s|self.path(\"user_settings.py\")|\"/${_configfile}\"|" ${_srcdir}/proton
sed -i "s|_proton=echo|_proton=/${_protondir}/proton|" ${srcdir}/launchers/proton.sh
sed -i "s|self.path(\"dist/share/default_pfx/\")|\"/${_pfxdir}/\"|" ${_srcdir}/proton
}

package() {
## create paths
install -d ${pkgdir}/${_protondir}/
install -d ${pkgdir}/${_licensedir}/
install -d --mode=2775 --group=games ${pkgdir}/${_pfxdir}/
chmod 0775 ${pkgdir}/${_pfxdir}/..
install -d ${pkgdir}/$(dirname ${_execfile})/
#install -d ${pkgdir}/$(dirname ${_configfile})/
## licenses
mv ${_srcdir}/LICENSE ${pkgdir}/${_licensedir}/license
mv ${_srcdir}/LICENSE.OFL ${pkgdir}/${_licensedir}/license_OFL
mv ${_srcdir}/protonfixes/LICENSE ${pkgdir}/${_licensedir}/license_protonfixes
## config files
#install --mode=0775 --group=games ${srcdir}/configs/user_settings.py ${pkgdir}/${_configfile}
## default pfx
mv ${_srcdir}/dist/share/default_pfx/* ${pkgdir}/${_pfxdir}
chown -R :games ${pkgdir}/${_pfxdir}
## executables
mv ${_srcdir}/* ${pkgdir}/${_protondir}
install --mode=0755 ${srcdir}/launchers/proton.sh ${pkgdir}/${_execfile}
}

# Maintainer: Matthias Blaicher <matthias at blaicher dot com>
#
# NOTE: If you plan on using the usbblaster make sure you are member of the plugdev group.
# NOTE: Altera has dramatically changed their packing in regards to version 12. This
#       PKGBUILD will install the full Altera suite now. Be aware that the space requirement
#       is around 13GB.
#
pkgname=quartus-lite
pkgver=17.1.1.593
pkgrel=1
pkgdesc="Quartus Prime Lite Edition design software for Altera FPGA's. Modular package"
arch=('x86_64')
url="http://dl.altera.com/?edition=lite"
license=('custom')

# base version for when an update needs to be applied
pkgverbase=17.1.0.590
_build_nr_base="${pkgverbase##*.}"
_build_nr="${pkgver##*.}"
_alteradir="/opt/altera"

# According to the installer script, these dependencies are needed for the installer
depends=('desktop-file-utils' 'expat' 'fontconfig' 'freetype2'
   'glibc' 'gtk2' 'libcanberra' 'libpng' 'libpng12' 'libice' 'libsm'
   'util-linux' 'ncurses' 'zlib' 'libx11' 'libxau'
   'libxdmcp' 'libxext' 'libxft' 'libxrender' 'libxt' 'libxtst')

#makedepends=('bash')

optdepends=('quartus-lite-max: MAXII, MAXV device support'
            'quartus-lite-max10: MAX10 device support'
            'quartus-lite-cyclone: Cyclone IV device support'
            'quartus-lite-cyclonev: Cyclone V device support'
            'quartus-lite-arria: Arria II device support'
            'quartus-lite-modelsim: ModelSim-Altera Edition'
           )
           # Pkgnames are taken directly from altera downloads file names

source=("http://download.altera.com/akdlm/software/acdsinst/${pkgver%.*.*}std.1/${_build_nr}/update/QuartusSetup-${pkgver}-linux.run"
  "http://download.altera.com/akdlm/software/acdsinst/${pkgverbase%.*.*}std/${_build_nr_base}/ib_installers/QuartusLiteSetup-${pkgverbase}-linux.run"
	"quartus.desktop" "51-usbblaster.rules" "quartus.install")
md5sums=('70e8016ea12cf7835dfcd3b22b1e3153'
         '8a22e65f15b695e7967a292caa7275f3'
         'd7181c4c6d88c7bd34061214ee350d73'
         'f5744dc4820725b93917e3a24df13da9'
         'a331a81c44aed062a7af6d28542c3d82')

#options=('strip' 'upx') # Stripping and UPX will takes ages, I'd avoid it.
install='quartus.install'
PKGEXT=".pkg.tar" # Do not compress

package() {
    cd "${srcdir}"

    # TODO: Make bogus $DISPLAY
    chmod a+x "QuartusLiteSetup-${pkgverbase}-linux.run"
    DISPLAY="" ./"QuartusLiteSetup-${pkgverbase}-linux.run" --mode unattended --unattendedmodeui none --accept_eula 1 --installdir "${pkgdir}/${_alteradir}"

    # Remove uninstaller and install logs since we have a working package management
    rm -r "${pkgdir}${_alteradir}/uninstall"
    rm -r "${pkgdir}${_alteradir}/logs"

    # Remove for now parts that are not needed for core quartus:
    # TODO: Split instead of removing

    # Nios2Eds - no comments on this abomination
    #rm -rf ${pkgdir}/${_alteradir}/nios2eds
    # Altera IP cores - you probably don't want to use them anyway (see opencores)
    #rm -rf ${pkgdir}/${_alteradir}/ip
    # HLS (HLD) - high level synthesis
    #rm -rf ${pkgdir}/${_alteradir}/hld
    # Help (Nearly 1GiB)
    #rm -rf ${pkgdir}/${_alteradir}/quartus/common/help

    # Replace altera directory in integration files
    sed -i.bak "s,_alteradir,$_alteradir,g" quartus.desktop

    # Copy license file
    install -D -m644 "${pkgdir}${_alteradir}/licenses/license.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

    # Install integration files
    install -D -m644 51-usbblaster.rules "${pkgdir}/etc/udev/rules.d/51-usbblaster.rules"
    install -D -m644 quartus.desktop "${pkgdir}/usr/share/applications/quartus.desktop"
}

# vim:set ts=2 sw=2 et:

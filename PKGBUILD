# Maintainer: Keshav P R <(the.ridikulus.rat) (aatt) (gemmaeiil) (ddoott) (ccoomm)>

# _GNU_EFI_LIB_DIR="/usr/lib"

_actualname="refind"
_pkgname="${_actualname}-efi-x86_64"
pkgname="${_pkgname}"

pkgver="0.4.2"
pkgrel="1"
pkgdesc="Rod Smith's fork of rEFIt UEFI Boot Manager"
url="http://www.rodsbooks.com/refind/index.html"
arch=('any')
license=('GPL3' 'custom')

makedepends=('gnu-efi>=3.0q')
depends=('dosfstools' 'efibootmgr')
optdepends=('mactel-boot: For bless command in Apple Mac systems')

replaces=('refind-x86_64')

backup=('boot/efi/EFI/arch/refind/refind.conf'
        'boot/efi/EFI/arch/refind/refind_linux.conf')

options=('!strip' 'docs')
install="${_pkgname}.install"

source=("http://downloads.sourceforge.net/refind/refind-src-${pkgver}.zip"
        'refind_include_more_shell_paths.patch'
        'refind_linux.conf')

sha256sums=('2d6f77139df9038c1431e4afb4a229a13300b8b579d5ecc61ef8ec923541637d'
            'db84334a7bea73c6843d82e9d02e1edbda65ff9806bc64e89b885c606c87e5b6'
            '9aac6e65018965ba182ec2d246d37fc5f9269ae96504956d8a51355c3ba1b62f')

build() {
	
	if [[ "${CARCH}" != "x86_64" ]]; then
		echo "${pkgname} package can be built only in a x86_64 system. Exiting."
		exit 1
	fi
	
	cd "${srcdir}/refind-${pkgver}/"
	echo
	
	make clean || true
	echo
	
	patch -Np1 -i "${srcdir}/refind_include_more_shell_paths.patch"
	echo
	
	sed 's|/usr/local/include/efi|/usr/include/efi|g' -i "${srcdir}/refind-${pkgver}/Make.common" || true
	sed 's|/usr/local/lib|/usr/lib|g' -i "${srcdir}/refind-${pkgver}/Make.common" || true
	echo
	
	make
	echo
	
}

package() {
	
	cd "${srcdir}/refind-${pkgver}/"
	
	## install the rEFInd x86_64 UEFI app
	install -d "${pkgdir}/boot/efi/EFI/arch/refind/"
	install -D -m0644 "${srcdir}/refind-${pkgver}/refind/refind_x64.efi" "${pkgdir}/boot/efi/EFI/arch/refind/refindx64.efi"
	
	## install the rEFInd config file
	install -D -m0644 "${srcdir}/refind-${pkgver}/refind.conf-sample" "${pkgdir}/boot/efi/EFI/arch/refind/refind.conf"
	install -D -m0644 "${srcdir}/refind_linux.conf" "${pkgdir}/boot/efi/EFI/arch/refind/refind_linux.conf"
	
	## install the rEFInd icons
	install -d "${pkgdir}/boot/efi/EFI/arch/refind/icons/"
	install -D -m0644 "${srcdir}/refind-${pkgver}/icons"/* "${pkgdir}/boot/efi/EFI/arch/refind/icons/"
	
	## install the rEFInd docs
	install -d "${pkgdir}/usr/share/refind/docs/html/"
	install -d "${pkgdir}/usr/share/refind/docs/Styles/"
	install -D -m0644 "${srcdir}/refind-${pkgver}/docs/refind"/* "${pkgdir}/usr/share/refind/docs/html/"
	install -D -m0644 "${srcdir}/refind-${pkgver}/docs/Styles"/* "${pkgdir}/usr/share/refind/docs/Styles/"
	install -D -m0644 "${srcdir}/refind-${pkgver}/README.txt" "${pkgdir}/usr/share/refind/docs/README.txt"
	install -D -m0644 "${srcdir}/refind-${pkgver}/NEWS.txt" "${pkgdir}/usr/share/refind/docs/NEWS.txt"
	rm -f "${pkgdir}/usr/share/refind/docs/html/.DS_Store" || true
	
	## install the rEFIt license file, since rEFInd is a fork of rEFIt
	install -d "${pkgdir}/usr/share/licenses/refind/"
	install -D -m0644 "${srcdir}/refind-${pkgver}/LICENSE.txt" "${pkgdir}/usr/share/licenses/refind/LICENSE"
	
}

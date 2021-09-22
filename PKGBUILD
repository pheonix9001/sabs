pkgname=('sabs')
arch=('any')
pkgver='0.5'
pkgrel=1

pkgdesc='A thin wrapper around the arch build system to make it OP'
license=('NONE')

# not an aur helper atm
provides=('sabs' 'abs-wrapper' 'aur-helper' \
	)

function package {
	cd ..
	mkdir -p "${pkgdir}/usr/bin"
	mkdir -p "${pkgdir}/usr/share/doc"

	cp -R src/* "${pkgdir}/usr/bin"
	cp -R sabs.conf "${pkgdir}/usr/share/doc/sabs.conf"
}

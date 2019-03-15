# Maintainer: Grey Christoforo <first name at last name dot net>

_appname=freecad
pkgname="${_appname}-git"
pkgver=0.18.r26.g51fcdd2c0
pkgrel=2
epoch=1
pkgdesc='A general purpose 3D CAD modeler - git checkout'
arch=('x86_64')
url='https://www.freecadweb.org/'
license=('LGPL')
depends=('boost-libs' 'curl' 'hicolor-icon-theme' 'libspnav' 'opencascade'
         'coin' 'libtheora' 'med' 'jsoncpp' 'xerces-c'
         'python2-netcdf4' 'python2-pivy' 'python2-pyside2'
         'qt5-svg' 'qt5-webkit')
makedepends=('git' 'boost' 'cmake' 'desktop-file-utils' 'pyside2-tools'
             'desktop-file-utils' 'eigen' 'gcc-fortran' 'swig')
optdepends=('python2-matplotlib'
            'pycollada-git: Create, edit and load COLLADA documents.')
provides=('freecad')
conflicts=('freecad')
source=("${pkgname}::git+https://github.com/FreeCAD/FreeCAD.git"
        "freecad.desktop"
	"freecad.xml")
md5sums=('SKIP'
         '7e781d41e90a1c137930e47672996a52'
         'c2f4154c8e4678825411de8e7fa54c6b')

pkgver() {
    cd "${srcdir}/${pkgname}"
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/_//'
}

build() {
    cd "${srcdir}/${pkgname}"

    cmake . \
          -DCMAKE_BUILD_TYPE=Release \
          -DCMAKE_INSTALL_PREFIX="/opt/${_appname}" \
          -DFREECAD_USE_OCC_VARIANT="Official Version" \
          -DPYTHON_EXECUTABLE=/usr/bin/python2 \
          -DBUILD_QT5=ON

    make
}

package() {
    cd "${srcdir}/${pkgname}"
    local bin="FreeCAD"
    local bin_cmd="FreeCADCmd"

    make DESTDIR="${pkgdir}" install
	
    # Symlink binaries to /usr/bin.
    mkdir -p "${pkgdir}/usr/bin"
    ln -s "/opt/${_appname}/bin/${bin}" "${pkgdir}/usr/bin/${bin}"
    ln -s "/opt/${_appname}/bin/${bin_cmd}" "${pkgdir}/usr/bin/${bin_cmd}"

    # Lowercase aliases like the official arch package.
    ln -s "/usr/bin/${bin}" "${pkgdir}/usr/bin/${bin,,}"
    ln -s "/usr/bin/${bin_cmd}" "${pkgdir}/usr/bin/${bin_cmd,,}"

    # Install pixmaps and desktop shortcut.
    for i in 16 32 48 64; do
        install -Dm644 "src/Gui/Icons/freecad-icon-${i}.png" \
            "${pkgdir}/usr/share/icons/hicolor/${i}x${i}/apps/${_appname}.png"
    done
    install -Dm644 "src/Gui/Icons/freecad.svg" \
        "${pkgdir}/usr/share/icons/hicolor/scalable/apps/${_appname}.svg"
    desktop-file-install \
        --dir="${pkgdir}/usr/share/applications" "${srcdir}/${_appname}.desktop"

    # Install mime info.
    install -D -m644 "${srcdir}/${_appname}.xml" \
        "${pkgdir}/usr/share/mime/packages/${_appname}.xml"
}

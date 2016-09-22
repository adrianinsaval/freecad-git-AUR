# Maintainer: Markus Hovorka <m.hovorka@live.de>
# Contributer: Christian Hesse <mail@eworm.de>

pkgname=freecad-git
pkgver=0.17pre.r1771.gf944ab3
pkgrel=1
epoch=1
pkgdesc='A general purpose 3D CAD modeler - git checkout'
arch=('i686' 'x86_64')
url='http://www.freecadweb.org/'
license=('LGPL')
depends=('boost-libs' 'curl' 'hicolor-icon-theme' 'libspnav' 'opencascade'
         'med' 'xerces-c' 'python2-pivy' 'python2-pyside' 'qtwebkit'
	 'libtheora' 'shared-mime-info' 'vtk-qt4' 'jsoncpp')
makedepends=('git' 'boost' 'cmake' 'coin' 'python2-pyside-tools'
             'desktop-file-utils' 'eigen' 'gcc-fortran' 'swig' 'patch')
optdepends=('python2-matplotlib'
            'pycollada-git: Create, edit and load COLLADA documents.')
provides=('freecad')
conflicts=('freecad')
install=freecad.install
source=("$pkgname::git+https://github.com/FreeCAD/FreeCAD.git"
	"freecad.install"
        "freecad.desktop"
	"freecad.xml"
	"fem-rpath.patch")
md5sums=('SKIP'
         '2fad48203f96f1e7cb97934ea20ed848'
         '0a4d0635dbd97d9f594ac8e927284316'
         'c2f4154c8e4678825411de8e7fa54c6b'
         '99a41687a9ba980eea86aee4345d9a1d')

pkgver() {
	cd "$pkgname"
	git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/_//'
}

prepare() {
	cd "$srcdir/$pkgname"
	patch -p1 -i "$srcdir/fem-rpath.patch"
}

build() {
	cd "$srcdir/$pkgname"

	cmake -DCMAKE_BUILD_TYPE=Release \
	      -DCMAKE_INSTALL_PREFIX:PATH="/opt/freecad" \
	      -DOCC_INCLUDE_DIR:PATH=/opt/opencascade/inc \
	      -DOCC_LIBRARY_DIR:PATH=/opt/opencascade/lib \
	      -DVTK_DIR:PATH=/opt/vtk-qt4/lib/cmake/vtk-7.0 \
	      -DPYTHON_EXECUTABLE:FILEPATH=/usr/bin/python2 \
	      -DPYSIDEUIC4BINARY:FILEPATH=/usr/bin/python2-pyside-uic

	make
}

package() {
	cd "$srcdir/$pkgname"

	make DESTDIR="$pkgdir" install
	
	# Symlink binaries to /usr/bin.
	mkdir -p "$pkgdir/usr/bin"
	ln -s "/opt/freecad/bin/FreeCAD" "$pkgdir/usr/bin/FreeCAD"
	ln -s "/opt/freecad/bin/FreeCADCmd" "$pkgdir/usr/bin/FreeCADCmd"

	# Lowercase aliases like the official arch package.
	ln -s "/opt/freecad/bin/FreeCAD" "$pkgdir/usr/bin/freecad"
	ln -s "/opt/freecad/bin/FreeCADCmd" "$pkgdir/usr/bin/freecadcmd"

	# Install pixmaps and desktop shortcut.
	desktop-file-install \
	  --dir="$pkgdir/usr/share/applications" "$srcdir/freecad.desktop"

	# Install mime info.
	install -D -m644 "$srcdir/freecad.xml" \
	  "$pkgdir/usr/share/mime/packages/freecad.xml"
}

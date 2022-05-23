# Maintainer: Adrian Insaurralde (adrianinsaval) <username at gmail dot com>

pkgname=freecad-linkdaily
pkgver=2022.05.20.edge.r0.gd47fafeec3
pkgrel=1
pkgdesc='An open source parametric 3D CAD modeler - LinkDaily branch by realthunder - installed to opt directory - git checkout'
arch=('x86_64')
url='https://github.com/realthunder/FreeCAD'
license=('LGPL')
depends=(
boost-libs
glew
jsoncpp
med
netcdf
opencascade
openmpi
pyside2
pyside2-tools
python-yaml
python-matplotlib
python-pivy
python-ply
qt5-svg
qt5-tools
qt5-webkit
qt5-webengine
qt5-webchannel
qt5-x11extras
qt5-xmlpatterns
qt5-base
shared-mime-info
xerces-c
vtk
)
makedepends=(
boost
cmake
coin
eigen
git
ninja
python-shiboken2
shiboken2
swig
gendesk
)
checkdepends=(
fmt 
pugixml
)
optdepends=(
'povray: ray tracing support'
'luxcorerender: ray tracing support'
'libspnav: 3d mouse support'
'openscad: OpenSCAD support'
'graphviz: dependency graph support'
'python-markdown: markdown support in addon manager'
'python-gitpython: support downloading addons with git'
)
source=("git+https://github.com/realthunder/FreeCAD.git#branch=LinkDaily")
md5sums=('SKIP')

pkgver() {
  cd FreeCAD
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/_//'
}

prepare() {
  # Create desktop shortcut
  gendesk -f -n --pkgname "freecad-link" --pkgdesc "$pkgdesc" --name "FreeCAD LinkDaily" \
    --mimetypes='application/x-extension-fcstd' --startupnotify=true
}

build() {
  cd FreeCAD

  cmake -Wno-dev -G Ninja -B build -S . \
    -D BUILD_ENABLE_CXX_STD=C++17 \
    -D BUILD_RAYTRACING=OFF \
    -D BUILD_ROBOT=OFF \
    -D BUILD_FLAT_MESH=ON \
    -D CMAKE_BUILD_TYPE=None \
    -D CMAKE_C_FLAGS="${CFLAGS} -fPIC -w" \
    -D CMAKE_CXX_FLAGS="${CXXFLAGS} -fPIC -w" \
    -D FREECAD_USE_EXTERNAL_PIVY=ON \
    -D FREECAD_USE_QT_FILEDIALOG=ON \
    -D PYTHON_EXECUTABLE=/usr/bin/python \
    -D CMAKE_INSTALL_PREFIX="/opt/freecad-link" \

  cmake --build build
}

check() {
  cd FreeCAD/build
  LD_LIBRARY_PATH=lib bin/FreeCADCmd --console --run-test 0
}

package() {
  cd FreeCAD
  DESTDIR="${pkgdir}" cmake --install build

  # package desktop shortcut
  install -Dm644 "${srcdir}/freecad-link.desktop" "${pkgdir}/usr/share/applications/freecad-link.desktop"
  install -d "${pkgdir}/usr/share/pixmaps"
  ln -s "${pkgdir}/opt/freecad-link/share/icons/hicolor/scalable/apps/freecad.svg" "${pkgdir}/usr/share/pixmaps/freecad-link.svg"

  # links for bin
  install -d "${pkgdir}/usr/bin"
  ln -s /opt/freecad-link/bin/FreeCAD "$pkgdir/usr/bin/freecad-link"
  ln -s /opt/freecad-link/bin/FreeCADCmd "$pkgdir/usr/bin/freecadcmd-link"
}

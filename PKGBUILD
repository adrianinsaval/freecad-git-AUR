# Maintainer: Adrian Insaurralde <adrianinsaval at gmail dot com>

pkgname=freecad-opt
pkgver=0.22.0.34522.gd8636dd058
pkgrel=1
pkgdesc='An open source parametric 3D CAD modeler - installed to opt directory - git checkout'
arch=('x86_64')
url='https://www.freecad.org/'
license=('LGPL')
depends=(
boost-libs
fmt
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
python-packaging
python-pivy
python-ply
qt5-svg
qt5-tools
qt5-webengine
qt5-webchannel
qt5-x11extras
qt5-xmlpatterns
qt5-base
shared-mime-info
vtk
verdict
xerces-c
yaml-cpp
)
makedepends=(
boost
cmake
coin
eigen
git
ninja
nlohmann-json
python-shiboken2
shiboken2
swig
gendesk
)
checkdepends=(
pugixml
)
optdepends=(
'povray: ray tracing support'
'luxcorerender: ray tracing support'
'libspnav: 3D mouse support'
'openscad: OpenSCAD support'
'graphviz: dependency graph support'
'python-pip: support installing python dependencies for addons'
'calculix-ccx: FEM solver backend'
)
#source=("git+https://github.com/FreeCAD/FreeCAD.git")
#md5sums=('SKIP')

pkgver() {
  cd FreeCAD
  read -d$'/n' -r major minor patch < <(grep -Po "set\(PACKAGE_VERSION_(MAJOR|MINOR|PATCH) \"\K[0-9]*" CMakeLists.txt) || true
  count=$((24266 + $(git rev-list --count d29fd7d..HEAD) ))
  hash=$(git rev-parse --short HEAD)
  printf "%d.%d.%d.%d.g%s" "$major" "$minor" "$patch" "$count" "$hash"
}

prepare() {
  ln -fs ../../FreeCAD

  # Create desktop shortcut
  gendesk -f -n --pkgname "$pkgname" --pkgdesc "$pkgdesc" --name FreeCAD-opt \
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
    -D CMAKE_INSTALL_PREFIX="/opt/freecad" \

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
  install -Dm644 "${srcdir}/freecad-opt.desktop" "${pkgdir}/usr/share/applications/freecad-opt.desktop"
  install -d "${pkgdir}/usr/share/pixmaps"
  ln -s "${pkgdir}/opt/freecad/share/icons/hicolor/scalable/apps/freecad.svg" "${pkgdir}/usr/share/pixmaps/freecad-opt.svg"

  # links for bin
  install -d "${pkgdir}/usr/bin"
  ln -s /opt/freecad/bin/FreeCAD "$pkgdir/usr/bin/freecad-opt"
  ln -s /opt/freecad/bin/FreeCADCmd "$pkgdir/usr/bin/freecadcmd-opt"
}

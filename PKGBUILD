# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=openvsp
pkgver=3.26.1
pkgrel=1
pkgdesc='OpenVSP allows the user to create a 3D model of an aircraft defined by
         common engineering parameters.'
arch=('i686' 'x86_64')
url='http://www.openvsp.org'
license=('NASA OPEN SOURCE AGREEMENT VERSION 1.3')
depends=('cblas'
         'code-eli'
         'cpptest'
         'eigen'
         'fltk'
         'freeglut'
         'gcc'
         'glew'
         'glm'
         'libxml2')
optdepends=('doxygen: generate documentation'
            'graphviz: generate documentation'
            'python: python API module'
            'swig: build interface to APIs')
makedepends=('cmake' 'unzip')
_name=OpenVSP-OpenVSP_${pkgver}
source=("${pkgname}-${pkgver}"::"https://github.com/OpenVSP/OpenVSP/archive/OpenVSP_${pkgver}.tar.gz"
        "stackgeom_ptr.patch"::"https://patch-diff.githubusercontent.com/raw/OpenVSP/OpenVSP/pull/219.patch")
sha256sums=('4870fe67da063293d6e0444a4b2272564406f202982025620b03a610b51c55de'
            'SKIP')

prepare() {
  cd "${srcdir}/${_name}"
  patch --strip=1 < "${srcdir}/stackgeom_ptr.patch"
}

build() {

  # Create a build directory
  mkdir -p "${srcdir}/build"
  mkdir -p "${srcdir}/buildlibs"

  cd "${srcdir}/buildlibs"

  cmake ../${_name}/Libraries \
        -DCMAKE_PREFIX_PATH='/usr' \
        -DVSP_USE_SYSTEM_CPPTEST=true \
        -DVSP_USE_SYSTEM_LIBXML2=true \
        -DVSP_USE_SYSTEM_EIGEN=true \
        -DVSP_USE_SYSTEM_CODEELI=true \
        -DVSP_USE_SYSTEM_FLTK=true \
        -DVSP_USE_SYSTEM_GLM=true \
        -DVSP_USE_SYSTEM_GLEW=true \
        -DVSP_USE_SYSTEM_CMINPACK=false \
        -DVSP_USE_SYSTEM_LIBIGES=false \
        -DVSP_USE_SYSTEM_STEPCODE=false \
        -DVSP_USE_SYSTEM_EXPRPARSE=false \
        -DVSP_USE_SYSTEM_TRIANGLE=false

  make

  cd "${srcdir}/build"

  cmake ../${_name}/src \
      -DVSP_LIBRARY_PATH="${srcdir}/buildlibs"

  make

  make package
}

package() {
  cd ${srcdir}/build
  unzip "OpenVSP-${pkgver}-Linux.zip"
  cd "OpenVSP-${pkgver}-Linux"

  # binary
  mkdir -p ${pkgdir}/usr/bin
  cp vsp vspaero vspscript vspslicer vspviewer ${pkgdir}/usr/bin/

  # misc
  mkdir -p ${pkgdir}/usr/share/${pkgname}
  cp README.md ${pkgdir}/usr/share/${pkgname}
  cp LICENSE ${pkgdir}/usr/share/${pkgname}
  cp -r CustomScripts ${pkgdir}/usr/share/${pkgname}
  cp -r airfoil ${pkgdir}/usr/share/${pkgname}
  cp -r matlab ${pkgdir}/usr/share/${pkgname}
  cp -r scripts ${pkgdir}/usr/share/${pkgname}
  cp -r textures ${pkgdir}/usr/share/${pkgname}
  cp -r vspaero_ex ${pkgdir}/usr/share/${pkgname}
}

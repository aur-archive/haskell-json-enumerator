# custom variables
_hkgname=json-enumerator
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-json-enumerator
pkgver=0.0.1.1
pkgrel=22
pkgdesc="Pure-Haskell utilities for dealing with JSON with the enumerator package."
url="http://github.com/snoyberg/json-enumerator"
license=("BSD3")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc=7.0.3-2"
         "sh"
         "haskell-blaze-builder=0.3.0.1"
         "haskell-blaze-builder-enumerator=0.2.0.3"
         "haskell-bytestring=0.9.1.10"
         "haskell-containers=0.4.0.0"
         "haskell-enumerator=0.4.14"
         "haskell-json-types=0.1"
         "haskell-text=0.11.0.5"
         "haskell-transformers=0.2.2.0")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
sha256sums=("f47c42460679a2c52cb35e1474e5955542f336a0c5ab87d8c2f92ace77ea244d")

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}

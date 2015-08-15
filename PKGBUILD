# Maintainer: Chuan Ji <jichuan89@gmail.com>

_target="arm-linux-gnueabi"
pkgname=cross-${_target}-gcc-base
pkgver=4.8.0
pkgrel=1
pkgdesc="The GNU Compiler Collection"
arch=(i686 x86_64)
license=('GPL' 'LGPL')
url="http://gcc.gnu.org"
depends=("cross-${_target}-binutils" 'libmpc' 'libelf')
options=('!libtool' '!emptydirs' 'zipman' 'docs' '!strip')
source=("ftp://gcc.gnu.org/pub/gcc/releases/gcc-${pkgver}/gcc-${pkgver}.tar.bz2")
md5sums=('e6040024eb9e761c3bea348d1fa5abb0')

build() {
  cd $srcdir/gcc-$pkgver

  export CPPFLAGS=
  export CFLAGS="-O2 -pipe"
  export CXXFLAGS="-O2 -pipe"

  [[ -d gcc-build ]] || mkdir gcc-build
  cd gcc-build

  [ $NOEXTRACT -eq 1 ] || ../configure --prefix=/usr \
	--program-prefix=${_target}- \
	--target=${_target} \
	--enable-obsolete \
	--host=$CHOST \
	--build=$CHOST \
	--disable-shared --disable-nls \
        --disable-threads \
        --enable-languages=c,c++ --enable-multilib \
	--with-local-prefix=/usr/lib/${_target} \
	--with-as=/usr/bin/${_target}-as --with-ld=/usr/bin/${_target}-ld \
	--enable-softfloat \
	--with-float=soft \
        --with-newlib \
        --enable-interwork \
        --enable-addons \
        --with-sysroot=/usr/$CHOST/${_target}

  make all-gcc all-target-libgcc
}

package() {
  cd $srcdir/gcc-$pkgver/gcc-build

  export CFLAGS="-O2 -pipe"
  export CXXFLAGS="-O2 -pipe"

  make DESTDIR=$pkgdir install-gcc install-target-libgcc

  rm -f $pkgdir/usr/share/man/man7/fsf-funding.7*
  rm -f $pkgdir/usr/share/man/man7/gfdl.7*
  rm -f $pkgdir/usr/share/man/man7/gpl.7*
  rm -rf $pkgdir/usr/share/info

  cp -r $pkgdir/usr/libexec/* $pkgdir/usr/lib/
  rm -rf $pkgdir/usr/libexec

  # strip it manually
  strip $pkgdir/usr/bin/* 2>/dev/null || true
  find $pkgdir/usr/lib -type f -exec /usr/bin/${_target}-strip --strip-unneeded {} \; 2>/dev/null || true
}

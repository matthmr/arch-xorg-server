# Maintainer: Alexander Baldeck <kth5@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=xorg-server
pkgver=1.4.2
pkgrel=1
_mesaver=7.0.3
pkgdesc="X.Org X servers"
arch=('i686' 'x86_64')
license=('custom')
url="http://xorg.freedesktop.org"
depends=('libxkbui' 'ncurses' 'libxxf86misc' 'libxfont'
         'libxaw' 'libxxf86vm' 'xcursor-themes'
	 'xkeyboard-config' 'xorg-server-utils' 'xorg-utils' 'libdrm>=2.3.0'
	 'xorg-fonts-misc' 'pixman>=0.10' 'xbitmaps')
makedepends=('pkgconfig' 'xf86driproto' 'xcmiscproto' 'xtrans' 'bigreqsproto'
             'xf86bigfontproto' 'resourceproto' 'evieext' 'damageproto>=1.1.0'
	     'compositeproto>=0.4' 'scrnsaverproto' 'libxres' 'xorg-util-macros'
	     'randrproto' 'glproto>=1.4.9' 'renderproto>=0.9.3' 'autoconf'
	     'automake' 'libtool' 'bison' 'flex' 'gcc' 'libxrender'
	     'libxfixes' 'quilt' 'mesa')
options=('!libtool')
provides=('x-server')
groups=('xorg')
install=xorg-server.install
source=(${url}/releases/individual/xserver/${pkgname}-${pkgver}.tar.bz2
	http://downloads.sourceforge.net/sourceforge/mesa3d/MesaLib-${_mesaver}.tar.bz2
	ftp://ftp.archlinux.org/other/xorg-server/xorg-server-patches-1.4.2-1.tar.bz2)
md5sums=('fa2915ae377f61c340a18ebef484b64b'
         'e6e6379d7793af40a6bc3ce1bace572e'
         'bc907d6d69b55b445c3046a4d7ecc8ef')

build() {
  cd ${startdir}/src/${pkgname}-${pkgver}
  quilt push -a || return 1
  libtoolize --force --copy || return 1
  aclocal || return 1
  autoconf || return 1
  automake --add-missing || return 1
  ./configure --prefix=/usr \
              --with-mesa-source=${startdir}/src/Mesa-${_mesaver} \
              --enable-ipv6 \
              --enable-dri \
              --disable-dmx \
              --enable-xvfb \
              --enable-xnest \
              --enable-composite \
              --enable-xcsecurity \
              --enable-xorg \
              --enable-xtrap \
              --enable-xevie \
	      --enable-xephyr \
              --enable-glx-tls \
              --enable-xorgcfg \
	      --enable-kdrive \
	      --disable-xfbdev \
	      --disable-xfake \
	      --disable-xsdl \
	      --disable-kdrive-vesa \
              --enable-install-setuid \
              --disable-xprint \
              --sysconfdir=/etc/X11 \
              --localstatedir=/var \
              --with-default-font-path=/usr/share/fonts/misc,/usr/share/fonts/100dpi:unscaled,/usr/share/fonts/75dpi:unscaled,/usr/share/fonts/TTF,/usr/share/fonts/Type1 \
              --with-rgb-path=/usr/share/X11/rgb \
              --with-xkb-path=/usr/share/X11/xkb \
              --with-xkb-output=/usr/share/X11/xkb/compiled \
              --with-dri-driver-path=/usr/lib/xorg/modules/dri \
              --disable-config-hal \
              --disable-static

  make || return 1
  make DESTDIR=${startdir}/pkg install || return 1

  install -m755 -d ${startdir}/pkg/etc/X11 || return 1
  install -m755 -d ${startdir}/pkg/var/lib || return 1
  mv ${startdir}/pkg/usr/share/X11/xkb/compiled ${startdir}/pkg/var/lib/xkb
  ln -sf /var/lib/xkb ${startdir}/pkg/usr/share/X11/xkb/compiled || return 1

  cd ${startdir}/pkg/usr/lib/xorg/modules/extensions
  mv libGLcore.so libGLcore.xorg
  mv libglx.so libglx.xorg

  # No longer needed, but as nvidia-utils mangles these files,
  # keep them like this so they can be restored
  mv ${startdir}/pkg/usr/lib/xorg/modules/libwfb.so \
     ${startdir}/pkg/usr/lib/xorg/modules/libwfb.so.1.4 || return 1

  install -m755 -d ${startdir}/pkg/usr/share/licenses/${pkgname}
  install -m644 ${startdir}/src/${pkgname}-${pkgver}/COPYING ${startdir}/pkg/usr/share/licenses/${pkgname}/ || return 1
}

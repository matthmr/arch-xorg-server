# Maintainer: Alexander Baldeck <kth5@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=xorg-server
pkgver=1.4.99.906
pkgrel=1
_mesaver=7.1-rc3
pkgdesc="X.Org X servers"
arch=('i686' 'x86_64')
license=('custom')
url="http://xorg.freedesktop.org"
depends=('libx11>=1.1.4-3' 'libxkbui>=1.0.2' 'libgl' 'libxfont>=1.3.3' 'openssl>=0.9.8h' 'libxxf86misc>=1.0.1' 'libpciaccess>=0.10.3' 'libxv>=1.0.4' 'libxaw>=1.0.4' 'pixman>=0.11.8' 'hal>=0.5.11' 'xcursor-themes' 'xkeyboard-config>=1.3' 'xorg-server-utils' 'xorg-utils' 'xorg-fonts-misc' 'xbitmaps')
makedepends=('glproto>=1.4.9' 'xf86driproto>=2.0.4' 'mesa>=7.1rc3' 'randrproto>=1.2.2' 'renderproto>=0.9.3' 'scrnsaverproto>=1.1.0' 'resourceproto>=1.0.2' 'fontsproto>=2.0.2' 'videoproto>=2.2.2' 'compositeproto>=0.4' 'trapproto>=3.4.3' 'resourceproto>=1.0.2' 'xineramaproto>=1.1.2' 'evieext>=1.0.2' 'libxkbfile>=1.0.5' 'libxmu>=1.0.4' 'libxpm>=3.5.7' 'libxxf86dga>=1.0.2' 'libxrender>=0.9.4' 'libxi>=1.1.3' 'dmxproto>=2.2.2')
options=('!libtool')
provides=('x-server')
groups=('xorg')
install=xorg-server.install
source=(${url}/releases/individual/xserver/${pkgname}-${pkgver}.tar.bz2
	xorg-redhat-die-ugly-pattern-die-die-die.patch)
md5sums=('d225476fea6721d9f64c3268e47d6c51'
         '1a336eb22e27cbf443ec5a2ecddfa93c')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}
  patch -Np3 -i ${srcdir}/xorg-redhat-die-ugly-pattern-die-die-die.patch || return 1
  ./configure --prefix=/usr \
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
	      --disable-dri2 \
              --enable-install-setuid \
              --disable-xprint \
              --sysconfdir=/etc/X11 \
              --localstatedir=/var \
              --with-default-font-path=/usr/share/fonts/misc,/usr/share/fonts/100dpi:unscaled,/usr/share/fonts/75dpi:unscaled,/usr/share/fonts/TTF,/usr/share/fonts/Type1 \
              --with-xkb-path=/usr/share/X11/xkb \
              --with-xkb-output=/var/lib/xkb \
              --with-dri-driver-path=/usr/lib/xorg/modules/dri \
              --enable-config-hal \
	      --enable-config-dbus \
              --disable-static

  make || return 1
  make DESTDIR=${pkgdir} install || return 1

  rm -rf ${pkgdir}/var/log || return 1

  install -m755 -d ${pkgdir}/etc/X11 || return 1
  install -m755 -d ${pkgdir}/var/lib/xkb || return 1

  # No longer needed, but as nvidia-utils mangles these files,
  # keep them like this so they can be restored
  mv ${pkgdir}/usr/lib/xorg/modules/libwfb.so \
     ${pkgdir}/usr/lib/xorg/modules/libwfb.so.1.4 || return 1

  # Needed for non-mesa drivers
  mv ${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so ${pkgdir}/usr/lib/xorg/modules/extensions/libglx.xorg

  install -m755 -d ${pkgdir}/usr/share/licenses/${pkgname}
  install -m644 COPYING ${pkgdir}/usr/share/licenses/${pkgname}/ || return 1
}

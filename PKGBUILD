# Maintainer: Alexander Baldeck <kth5@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=xorg-server
pkgver=1.5.1
pkgrel=1
pkgdesc="X.Org X servers"
arch=('i686' 'x86_64')
license=('custom')
url="http://xorg.freedesktop.org"
depends=('libx11>=1.1.5' 'libxkbui>=1.0.2' 'libgl' 'libxfont>=1.3.3' 'openssl>=0.9.8h' 'libxxf86misc>=1.0.1' 'libpciaccess>=0.10.3' 'libxv>=1.0.4' 'libxaw>=1.0.4' 'pixman>=0.11.10' 'hal>=0.5.11' 'xcursor-themes' 'xkeyboard-config>=1.3' 'xorg-server-utils' 'xorg-utils' 'xorg-fonts-misc' 'xbitmaps' 'libdrm>=2.3.1')
makedepends=('glproto>=1.4.9' 'xf86driproto>=2.0.4' 'mesa>=7.2' 'randrproto>=1.2.2' 'renderproto>=0.9.3' 'scrnsaverproto>=1.1.0' 'fontsproto>=2.0.2' 'videoproto>=2.2.2' 'compositeproto>=0.4' 'trapproto>=3.4.3' 'resourceproto>=1.0.2' 'xineramaproto>=1.1.2' 'evieext>=1.0.2' 'libxkbfile>=1.0.5' 'libxmu>=1.0.4' 'libxpm>=3.5.7' 'libxxf86dga>=1.0.2' 'inputproto>=1.4.4' 'xcmiscproto>=1.1.2' 'bigreqsproto')
options=('!libtool')
provides=('x-server')
groups=('xorg')
install=xorg-server.install
source=(${url}/releases/individual/xserver/${pkgname}-${pkgver}.tar.bz2
	001_fedora_extramodes.patch
	xorg-redhat-die-ugly-pattern-die-die-die.patch)
md5sums=('d16f5a033f001c9069b4141194614da2'
         '033427f2b406cba8dd6103ff374e7156'
         '1a336eb22e27cbf443ec5a2ecddfa93c')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}
  # extramodes
  patch -Np1 -i ${srcdir}/001_fedora_extramodes.patch || return 1

  # Get rid of the ugly pattern
  patch -Np3 -i ${srcdir}/xorg-redhat-die-ugly-pattern-die-die-die.patch || return 1

  # Fix dbus config path
  sed -i -e 's/\$(sysconfdir)/\/etc/' config/Makefile.*  || return 1

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
              --enable-install-setuid \
              --enable-config-hal \
	      --enable-config-dbus \
	      --disable-xfbdev \
	      --disable-xfake \
	      --disable-xsdl \
	      --disable-kdrive-vesa \
	      --disable-dri2 \
              --disable-xprint \
              --disable-static \
              --sysconfdir=/etc/X11 \
              --localstatedir=/var \
              --with-default-font-path=/usr/share/fonts/misc,/usr/share/fonts/100dpi:unscaled,/usr/share/fonts/75dpi:unscaled,/usr/share/fonts/TTF,/usr/share/fonts/Type1 \
              --with-xkb-path=/usr/share/X11/xkb \
              --with-xkb-output=/var/lib/xkb \
              --with-dri-driver-path=/usr/lib/xorg/modules/dri || return 1

  make || return 1
  make DESTDIR=${pkgdir} install || return 1

  rm -rf ${pkgdir}/var/log || return 1

  install -m755 -d ${pkgdir}/etc/X11 || return 1
  install -m755 -d ${pkgdir}/var/lib/xkb || return 1

  # No longer needed, but as nvidia-utils mangles these files,
  # keep them like this so they can be restored
  mv ${pkgdir}/usr/lib/xorg/modules/libwfb.so \
     ${pkgdir}/usr/lib/xorg/modules/libwfb.so.1.4 || return 1

  # Needed for non-mesa drivers, libgl will restore it
  mv ${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so ${pkgdir}/usr/lib/xorg/modules/extensions/libglx.xorg

  install -m755 -d ${pkgdir}/usr/share/licenses/${pkgname}
  install -m644 COPYING ${pkgdir}/usr/share/licenses/${pkgname}/ || return 1
}

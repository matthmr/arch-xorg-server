# Maintainer: Alexander Baldeck <kth5@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=xorg-server
pkgver=1.6.1.901
pkgrel=3
pkgdesc="X.Org X servers"
arch=('i686' 'x86_64')
license=('custom')
url="http://xorg.freedesktop.org"
depends=('hal>=0.5.11' 'libgl' 'libxfont>=1.4.0' 'openssl>=0.9.8j' 'libpciaccess>=0.10.5' 'libxv>=1.0.4' 'pixman>=0.14.0' 'xcursor-themes' 'xkeyboard-config>=1.6' 'xorg-server-utils' 'xorg-fonts-misc' 'xbitmaps' 'diffutils' 'xf86-input-evdev>=2.2.2')
makedepends=('libx11>=1.2.1' 'mesa>=7.4.4' 'xf86driproto>=2.0.4' 'xtrans>=1.2.3' 'libxkbfile>=1.0.5' 'randrproto>=1.3.0' 'renderproto>=0.9.3' 'xcmiscproto>=1.1.2' 'bigreqsproto>=1.0.2' 'resourceproto>=1.0.2' 'videoproto>=2.2.2' 'compositeproto>=0.4' 'scrnsaverproto>=1.1.0' 'libxinerama>=1.0.3' 'xf86dgaproto>=2.0.3' 'recordproto>=1.13.2' 'libgl>=7.4.4' 'glproto>=1.4.9')
conflicts=('catalyst-utils<=9.2')
options=('!libtool')
provides=('x-server')
groups=('xorg')
install=xorg-server.install
source=(${url}/releases/individual/xserver/${pkgname}-${pkgver}.tar.bz2
        xorg-redhat-die-ugly-pattern-die-die-die.patch
        server-1.6-branch.patch
      	pending-1.6.2.patch)
md5sums=('e6cba1f07006143daa95ce3f11d999b2'
         '1a336eb22e27cbf443ec5a2ecddfa93c'
         '4137ebc011080b0d7afa271376653ab7'
         '06f41b42664b6dc9b56abdc0858901e4')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  # Get rid of the ugly pattern
  patch -Np3 -i "${srcdir}/xorg-redhat-die-ugly-pattern-die-die-die.patch" || return 1

  # Upstream updates from server-1.6 branch
  # DRI2 parts removed
  patch -Np1 -i "${srcdir}/server-1.6-branch.patch" || return 1

  # Pending updates for 1.6.2 - http://wiki.x.org/wiki/Server16Branch
  patch -Np1 -i "${srcdir}/pending-1.6.2.patch" || return 1

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
      	      --enable-xephyr \
              --enable-glx-tls \
	            --enable-kdrive \
              --enable-install-setuid \
              --enable-config-hal \
      	      --enable-config-dbus \
      	      --enable-record \
      	      --disable-xfbdev \
      	      --disable-xfake \
      	      --disable-xsdl \
              --disable-static \
              --sysconfdir=/etc/X11 \
              --localstatedir=/var \
              --with-default-font-path=/usr/share/fonts/misc,/usr/share/fonts/100dpi:unscaled,/usr/share/fonts/75dpi:unscaled,/usr/share/fonts/TTF,/usr/share/fonts/Type1 \
              --with-xkb-path=/usr/share/X11/xkb \
              --with-xkb-output=/var/lib/xkb \
              --with-dri-driver-path=/usr/lib/xorg/modules/dri || return 1

  make || return 1
  make DESTDIR="${pkgdir}" install || return 1

  rm -rf "${pkgdir}/var/log" || return 1

  install -m755 -d "${pkgdir}/etc/X11" || return 1
  install -m755 -d "${pkgdir}/var/lib/xkb" || return 1

  # Needed for non-mesa drivers, libgl will restore it
  mv "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so" \
     "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.xorg" || return 1

  install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/" || return 1
}

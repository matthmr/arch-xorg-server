# Maintainer: AndyRTR <andyrtr@archlinux.org>
# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=xorg-server
pkgname=('xorg-server' 'xorg-server-xephyr' 'xorg-server-xdmx' 'xorg-server-xvfb' 'xorg-server-xnest' 'xorg-server-xwayland' 'xorg-server-common' 'xorg-server-devel')
pkgver=1.18.2
pkgrel=3
arch=('i686' 'x86_64')
license=('custom')
groups=('xorg')
url="http://xorg.freedesktop.org"
makedepends=('pixman' 'libx11' 'mesa' 'mesa-libgl' 'xf86driproto' 'xcmiscproto' 'xtrans' 'bigreqsproto' 'randrproto' 
             'inputproto' 'fontsproto' 'videoproto' 'presentproto' 'compositeproto' 'recordproto' 'scrnsaverproto'
             'resourceproto' 'xineramaproto' 'libxkbfile' 'libxfont' 'renderproto' 'libpciaccess' 'libxv'
             'xf86dgaproto' 'libxmu' 'libxrender' 'libxi' 'dmxproto' 'libxaw' 'libdmx' 'libxtst' 'libxres'
             'xorg-xkbcomp' 'xorg-util-macros' 'xorg-font-util' 'glproto' 'dri2proto' 'libgcrypt' 'libepoxy'
             'xcb-util' 'xcb-util-image' 'xcb-util-renderutil' 'xcb-util-wm' 'xcb-util-keysyms' 'dri3proto'
             'libxshmfence' 'libunwind')
source=(${url}/releases/individual/xserver/${pkgbase}-${pkgver}.tar.bz2{,.sig}
        xvfb-run
        xvfb-run.1
        0001-glamor-swizzle-RED-to-0-for-alpha-textures.patch
        0002-Xext-vidmode-Reduce-verbosity-of-GetModeLine.patch
        0003-Revert-present-Requeue-if-flip-driver-hook-fails-and.patch)
validpgpkeys=('7B27A3F1A6E18CD9588B4AE8310180050905E40C'
              'C383B778255613DFDB409D91DB221A6900000011'
              'DD38563A8A8224537D1F90E45B8A2D50A0ECD0D3')
sha256sums=('022142b07f6477d140dcc915902df326408a53ca3a352426a499f142b25d632d'
            'SKIP'
            'ff0156309470fc1d378fd2e104338020a884295e285972cc88e250e031cc35b9'
            '2460adccd3362fefd4cdc5f1c70f332d7b578091fb9167bf88b5f91265bbd776'
            '10c66c10f4f71930e2ac3f6e07881e228ca88542af449d2c69c7744ec87335df'
            '72755a652e72144e3f28c8fa959b4a6df5def838db3cde5077a626e97baab591'
            '692cccb82ae20be237ac9fda9347c06952e9ca2971d63dc54cf63ad2223d41a0')

prepare() {
  cd "${pkgbase}-${pkgver}"

  # Fix red tint in Firefox
  patch -Np1 -i ../0001-glamor-swizzle-RED-to-0-for-alpha-textures.patch
  # Fix flooding of Xorg log file
  patch -Np1 -i ../0002-Xext-vidmode-Reduce-verbosity-of-GetModeLine.patch
  # workaround FS#48549
  # upstream https://bugs.freedesktop.org/show_bug.cgi?id=94515
  # upstream https://bugs.freedesktop.org/show_bug.cgi?id=94596
  patch -Np1 -i ../0003-Revert-present-Requeue-if-flip-driver-hook-fails-and.patch
}

build() {
  cd "${pkgbase}-${pkgver}"
  ./configure --prefix=/usr \
      --enable-ipv6 \
      --enable-dri \
      --enable-dmx \
      --enable-xvfb \
      --enable-xnest \
      --enable-composite \
      --enable-xcsecurity \
      --enable-libunwind \
      --enable-xorg \
      --enable-xephyr \
      --enable-glamor \
      --enable-xwayland \
      --enable-kdrive \
      --enable-kdrive-kbd \
      --enable-kdrive-mouse \
      --enable-config-udev \
      --enable-systemd-logind \
      --enable-suid-wrapper \
      --disable-install-setuid \
      --enable-record \
      --disable-xfbdev \
      --disable-xfake \
      --disable-static \
      --libexecdir=/usr/lib/xorg-server \
      --sysconfdir=/etc \
      --localstatedir=/var \
      --with-xkb-path=/usr/share/X11/xkb \
      --with-xkb-output=/var/lib/xkb \
      --with-fontrootdir=/usr/share/fonts \
      --with-sha1=libgcrypt
      
#      --without-dtrace \
#      --disable-linux-acpi --disable-linux-apm \

  make

  # Disable subdirs for make install rule to make splitting easier
  sed -e 's/^DMX_SUBDIRS =.*/DMX_SUBDIRS =/' \
      -e 's/^XVFB_SUBDIRS =.*/XVFB_SUBDIRS =/' \
      -e 's/^XNEST_SUBDIRS =.*/XNEST_SUBDIRS = /' \
      -e 's/^KDRIVE_SUBDIRS =.*/KDRIVE_SUBDIRS =/' \
      -e 's/^XWAYLAND_SUBDIRS =.*/XWAYLAND_SUBDIRS =/' \
      -i hw/Makefile
}

package_xorg-server-common() {
  pkgdesc="Xorg server common files"
  depends=('xkeyboard-config' 'xorg-xkbcomp' 'xorg-setxkbmap' 'xorg-fonts-misc'
           'libunwind')

  cd "${pkgbase}-${pkgver}"
  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-common"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/xorg-server-common"
  
  make -C xkb DESTDIR="${pkgdir}" install-data

  install -m755 -d "${pkgdir}/usr/share/man/man1"
  install -m644 man/Xserver.1 "${pkgdir}/usr/share/man/man1/"

  install -m755 -d "${pkgdir}/usr/lib/xorg"
  install -m644 dix/protocol.txt "${pkgdir}/usr/lib/xorg/"
}

package_xorg-server() {
  pkgdesc="Xorg X server"
  depends=(libepoxy libxdmcp libxfont libpciaccess libdrm pixman libgcrypt libxau xorg-server-common libxshmfence libgl xf86-input-driver)

  # see xorg-server-*/hw/xfree86/common/xf86Module.h for ABI versions - we provide major numbers that drivers can depend on
  # and /usr/lib/pkgconfig/xorg-server.pc in xorg-server-devel pkg
  provides=('X-ABI-VIDEODRV_VERSION=20' 'X-ABI-XINPUT_VERSION=22.1' 'X-ABI-EXTENSION_VERSION=9.0' 'x-server')
  conflicts=('nvidia-utils<=331.20' 'glamor-egl' 'xf86-video-modesetting')
  replaces=('glamor-egl' 'xf86-video-modesetting')
  install=xorg-server.install

  cd "${pkgbase}-${pkgver}"
  make DESTDIR="${pkgdir}" install
  
  # distro specific files must be installed in /usr/share/X11/xorg.conf.d
  install -m755 -d "${pkgdir}/etc/X11/xorg.conf.d"
  
  # Needed for non-mesa drivers, libgl will restore it
  mv "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so" \
     "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.xorg"

  rm -rf "${pkgdir}/var"

  rm -f "${pkgdir}/usr/share/man/man1/Xserver.1"
  rm -f "${pkgdir}/usr/lib/xorg/protocol.txt"

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server/COPYING"

  rm -rf "${pkgdir}/usr/lib/pkgconfig"
  rm -rf "${pkgdir}/usr/include"
  rm -rf "${pkgdir}/usr/share/aclocal"
}

package_xorg-server-xephyr() {
  pkgdesc="A nested X server that runs as an X application"
  depends=(libxfont libgl libepoxy libgcrypt libxv pixman xorg-server-common 'xcb-util-image'
           'xcb-util-renderutil' 'xcb-util-wm' 'xcb-util-keysyms')

  cd "${pkgbase}-${pkgver}/hw/kdrive"
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xephyr"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xephyr/COPYING"
}

package_xorg-server-xvfb() {
  pkgdesc="Virtual framebuffer X server"
  depends=(libxfont libxdmcp libxau libgcrypt pixman xorg-server-common xorg-xauth libgl)

  cd "${pkgbase}-${pkgver}/hw/vfb"
  make DESTDIR="${pkgdir}" install

  install -m755 "${srcdir}/xvfb-run" "${pkgdir}/usr/bin/"
  install -m644 "${srcdir}/xvfb-run.1" "${pkgdir}/usr/share/man/man1/"

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xvfb"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xvfb/COPYING"
}

package_xorg-server-xnest() {
  pkgdesc="A nested X server that runs as an X application"
  depends=(libxfont libxext libgcrypt pixman xorg-server-common libsystemd)

  cd "${pkgbase}-${pkgver}/hw/xnest"
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xnest"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xnest/COPYING"
}

package_xorg-server-xdmx() {
  pkgdesc="Distributed Multihead X Server and utilities"
  depends=(libxfont libxi libgcrypt libxaw libxrender libdmx libxfixes pixman xorg-server-common)

  cd "${pkgbase}-${pkgver}/hw/dmx"
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xdmx"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xdmx/COPYING"
}

package_xorg-server-xwayland() {
  pkgdesc="run X clients under wayland"
  depends=(libxfont libepoxy libgl pixman xorg-server-common)

  cd "${pkgbase}-${pkgver}/hw/xwayland"
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xwayland"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xwayland/COPYING"
}

package_xorg-server-devel() {
  pkgdesc="Development files for the X.Org X server"
  depends=(# see pkgdir/usr/lib/pkgconfig/xorg-server.pc
           xproto randrproto renderproto xextproto inputproto kbproto 
           fontsproto pixman videoproto xf86driproto glproto 
           mesa dri2proto dri3proto xineramaproto libpciaccess
           resourceproto scrnsaverproto presentproto
           # not technically required but almost every Xorg pkg needs it to build
           xorg-util-macros)

  cd "${pkgbase}-${pkgver}"
  make DESTDIR="${pkgdir}" install

  rm -rf "${pkgdir}/usr/bin"
  rm -rf "${pkgdir}/usr/share/man"
  rm -rf "${pkgdir}/usr/share/doc"
  rm -rf "${pkgdir}/usr/share/X11"
  rm -rf "${pkgdir}/usr/lib/xorg"
  rm -rf "${pkgdir}/usr/lib/xorg-server"
  rm -rf "${pkgdir}/var"

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-devel"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-devel/COPYING"
}

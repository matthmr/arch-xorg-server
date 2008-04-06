# $Id: PKGBUILD,v 1.53 2008/03/23 11:44:58 jgc Exp $
# Maintainer: Alexander Baldeck <kth5@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=xorg-server
pkgver=1.4.0.90
pkgrel=9
_mesaver=7.0.3-rc2
pkgdesc="X.Org X servers"
arch=('i686' 'x86_64')
license=('custom')
url="http://xorg.freedesktop.org"
depends=('libxkbui' 'ncurses' 'libxi' 'libxxf86misc' 'libxfont'
         'libxrender' 'libdmx' 'libxaw' 'libxxf86vm' 'xcursor-themes'
	 'xkeyboard-config' 'xorg-server-utils' 'xorg-utils' 'libdrm>=2.3.0'
	 'libxfixes' 'xorg-fonts-misc' 'pixman>=0.9.6' 'xbitmaps' 'libgl')
makedepends=('pkgconfig' 'xf86driproto' 'xcmiscproto' 'xtrans' 'bigreqsproto'
             'xf86bigfontproto' 'resourceproto' 'evieext' 'damageproto>=1.1.0'
	     'compositeproto>=0.4' 'scrnsaverproto' 'libxres' 'xorg-util-macros'
	     'randrproto' 'glproto>=1.4.9' 'renderproto>=0.9.3' 'autoconf'
	     'automake' 'libtool' 'bison' 'flex' 'gcc')
options=('!libtool')
provides=('x-server')
groups=('xorg')
install=xorg-server.install
source=(${url}/releases/individual/xserver/${pkgname}-${pkgver}.tar.bz2
	http://www.mesa3d.org/beta/MesaLib-7.0.3-rc2.tar.gz
	ftp://ftp.archlinux.org/other/xorg-server/xorg-server-patches-1.4-8.tar.bz2
	xkb-fixes.patch)

md5sums=('bb16e969850dbb5d3805cb88d35656d0'
         '141f5c1c89b090d0f0bd92522ebb8de9'
         '55341205777a42712c445c5e24b57b6a'
	 '11fb627cd9004822050ef13a6d27f9a0')

build() {
  cd ${startdir}/src/${pkgname}-${pkgver}
  patch -Np1 -i ${startdir}/src/xorg-server-1.4-git20080227.patch || return 1
  patch -Np1 -i ${startdir}/src/001_ubuntu_add_extra_modelines_from_xorg.patch || return 1
  patch -Np1 -i ${startdir}/src/02_libvgahw_gcc4_volatile_fix.diff || return 1
  patch -Np1 -i ${startdir}/src/05_kill_type1.diff || return 1
  patch -Np1 -i ${startdir}/src/06_use_proc_instead_of_sysfs_for_pci_domains.diff || return 1
  patch -Np0 -i ${startdir}/src/10_dont_look_in_home_for_config.diff || return 1
  patch -Np1 -i ${startdir}/src/11_dont_crash_on_bad_dri_mode.diff || return 1
  patch -Np1 -i ${startdir}/src/14_default_screen_section.diff || return 1
  patch -Np1 -i ${startdir}/src/21_glx_align_fixes.patch || return 1
  patch -Np1 -i ${startdir}/src/40_default_dpi_96.patch || return 1
  patch -Np1 -i ${startdir}/src/41_vbe_filter_less.diff || return 1
  patch -Np1 -i ${startdir}/src/43_allow_override_BIOS_EDID_preferred_mode.diff || return 1
  patch -Np1 -i ${startdir}/src/44_preferredmode_infinite_loop.diff || return 1
  patch -Np1 -i ${startdir}/src/45_only_XF86_APM_CAPABILITY_CHANGED_for_video_change_acpi_events.diff || return 1
  patch -Np1 -i ${startdir}/src/46_reduce_wakeups_from_smart_scheduler.patch || return 1
  patch -Np1 -i ${startdir}/src/51_xkb-and-loathing.diff || return 1
  patch -Np1 -i ${startdir}/src/101_fedora-apm-typedefs.patch || return 1
  patch -Np1 -i ${startdir}/src/102_ubuntu_sharevts_load_cpu.patch || return 1
  patch -Np1 -i ${startdir}/src/104_fedora_init_origins_fix.patch || return 1
  patch -Np1 -i ${startdir}/src/107_fedora_dont_backfill_bg_none.patch || return 1
  patch -Np1 -i ${startdir}/src/108_fedora_honor_displaysize.patch || return 1
  patch -Np1 -i ${startdir}/src/109_glx_fail_if_no_texture_bound.diff || return 1
  patch -Np1 -i ${startdir}/src/110_fedora_no_move_damage.patch || return 1
  patch -Np1 -i ${startdir}/src/120_fedora_xserver-xaa-evict-pixmaps.patch || return 1
  patch -Np1 -i ${startdir}/src/121_only_switch_vt_when_active.diff || return 1
  patch -Np1 -i ${startdir}/src/139_fedora_xserver-1.3.0-document-fontpath-correctly.patch || return 1
  patch -Np1 -i ${startdir}/src/142_fedora_xserver-1.3.0-no-pseudocolor-composite.patch || return 1
  patch -Np1 -i ${startdir}/src/144_fedora_xserver-1.3.0-xnest-exposures.patch || return 1
  patch -Np1 -i ${startdir}/src/148_dix_touchscreen_fixes.diff || return 1
  patch -Np1 -i ${startdir}/src/149_add_quirks_for_physical_screen_size_issues.patch || return 1
  patch -Np1 -i ${startdir}/src/150_edid_quirk_lp154w01.patch || return 1
  patch -Np1 -i ${startdir}/src/152_xserver_exa_force_greedy.patch || return 1
  patch -Np1 -i ${startdir}/src/153_exa_skip_empty_glyphs.diff || return 1
  patch -Np1 -i ${startdir}/src/155_exa_fix_off-by-one.diff || return 1
  patch -Np1 -i ${startdir}/src/mesa-7.0.3-rc2.patch || return 1
  patch -Np3 -i ${startdir}/src/xorg-redhat-die-ugly-pattern-die-die-die.patch || return 1

  patch -Np1 -i ${startdir}/src/xkb-fixes.patch || return 1

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

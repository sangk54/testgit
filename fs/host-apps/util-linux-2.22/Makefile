#$L$
# Copyright (C) 2016 Ridgerun (http://www.ridgerun.com).
#$L$

PKG_URL=https://www.kernel.org/pub/linux/utils/util-linux/v2.22/
PKG_TARBALL=util-linux-2.22.tar.gz
PKG_SHA1SUM=c14597997d62843e49612a46a0b11098db120bd1

LIBRARIES= usr/lib/{libblkid.so.1.1.0,libmount.so.1.1.0,libuuid.so.1.3.0}
BINARIES=sfdisk
AUTOTOOLS_PARAMS= \
	--disable-makeinstall-chown \
	--without-ncurses \
	--disable-agetty \
	--disable-cramfs \
	--disable-eject \
	--disable-fallocate \
	--disable-fsck \
	--disable-kill \
	--disable-login \
	--disable-losetup \
	--disable-mount \
	--disable-mountpoint \
	--disable-partx \
	--disable-pivot_root \
	--disable-rename \
	--disable-schedutils \
	--disable-su \
	--disable-sulogin \
	--disable-switch_root \
	--disable-unshare \
	--disable-wall

include ../../../bsp/classes/rrsdk_local.class
include $(CLASSES)/autotools_local.class

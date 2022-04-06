# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2006-2021 OpenWrt.org

include $(TOPDIR)/rules.mk

ARCH:=armeb
BOARD:=ixp4xx
BOARDNAME:=Intel XScale IXP4xx
FEATURES:=squashfs
CPU_TYPE:=xscale

KERNEL_PATCHVER:=5.18

define Target/Description
	Build firmware images for the IXP4xx XScale CPU
endef

KERNELNAME:=zImage dtbs

include $(INCLUDE_DIR)/target.mk

DEFAULT_PACKAGES += ixp4xx-microcode fconfig

$(eval $(call BuildTarget))

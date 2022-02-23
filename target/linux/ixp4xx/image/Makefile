# SPDX-License-Identifier: GOPPL-2.0-only
#
# Copyright (C) 2006-2021 OpenWrt.org

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

define Image/BuildKernel
	cp $(KDIR)/zImage $(BIN_DIR)/$(IMG_PREFIX)-zImage
	BIN_DIR=$(BIN_DIR) IMG_PREFIX="$(IMG_PREFIX)" $(TOPDIR)/scripts/arm-magic.sh
endef

define Image/BuildKernel/Initramfs
	cp $(KDIR)/zImage-initramfs $(BIN_DIR)/$(IMG_PREFIX)-initramfs-zImage
	BIN_DIR=$(BIN_DIR) IMG_PREFIX="$(IMG_PREFIX)-initramfs" $(TOPDIR)/scripts/arm-magic.sh
endef

# Build sysupgrade image
define BuildFirmware/Generic
	dd if=$(KDIR)/zImage of=$(KDIR)/zImage.pad bs=64k conv=sync; \
	dd if=$(KDIR)/root.$(1) of=$(KDIR)/root.$(1).pad bs=128k conv=sync; \
	sh $(TOPDIR)/scripts/combined-image.sh \
		$(KDIR)/zImage.pad \
		$(KDIR)/root.$(1).pad \
		$(BIN_DIR)/$(IMG_PREFIX)-$(patsubst jffs2-%,jffs2,$(patsubst squashfs-%,squashfs,$(1)))-sysupgrade.bin
endef

define Image/Build
	$(call Image/Build/$(1),$(1))
	$(call BuildFirmware/Generic,$(1))
endef

# All DTB files are prefixed with "gemini-"
define Device/Default
	PROFILES := Default
	KERNEL_DEPENDS = $$(wildcard $(DTS_DIR)/$$(DEVICE_DTS).dts)
	KERNEL_NAME := zImage
	KERNEL := kernel-bin | append-dtb
	BLOCKSIZE := 128k
endef

define Device/cambria
	DEVICE_VENDOR := Gateway
	DEVICE_MODEL := GW2358-4
	DEVICE_DTS := intel-ixp43x-gateworks-gw2358
	KERNEL := kernel-bin | append-dtb
	IMAGES := factory.bin
	IMAGE/factory.bin := append-kernel | append-rootfs | pad-rootfs
endef
TARGET_DEVICES += cambria

define Device/watchguard
	DEVICE_VENDOR := Watchguard
	DEVICE_MODEL := XTM21-W
	DEVICE_DTS := intel-ixp43x-watchguard-xtm21w
	KERNEL := kernel-bin | append-dtb
	IMAGES := factory.bin
	IMAGE/factory.bin := append-kernel | append-rootfs | pad-rootfs
endef
TARGET_DEVICES += watchguard


$(eval $(call BuildImage))

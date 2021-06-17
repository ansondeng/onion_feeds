#
# Copyright (C) 2014-2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8812cu
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/ansondeng/rtl8812cu.git
PKG_SOURCE_DATE:=2020-6-9
PKG_SOURCE_VERSION:=d39446d89246e4c3b500236ccda5c5c3c7b6a189
PKG_MAINTAINER:=ansondeng <ansondeng3000@gmail.com>

PKG_BUILD_PARALLEL:=1

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/$(PKG_NAME)
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek 8812AU/8821AU USB WiFi driver
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT @!LINUX_3_18 @!LINUX_4_9
  FILES:=$(PKG_BUILD_DIR)/$(PKG_NAME).ko
  AUTOLOAD:=$(call AutoProbe,$(PKG_NAME))
  PROVIDES:=kmod-$(PKG_NAME)
endef

define KernelPackage/$(PKG_NAME)/description
  Realtek 8812AU/8821AU USB WiFi driver for AC1200 (801.11ac) Wireless Dual-Band USB Adapter
endef

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		SUBDIRS="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

define KernelPackage/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/hotplug.d/usb
	$(INSTALL_BIN) ./files/wifi.hotplug $(1)/etc/hotplug.d/usb/30-wifi
endef

$(eval $(call KernelPackage,$(PKG_NAME)))

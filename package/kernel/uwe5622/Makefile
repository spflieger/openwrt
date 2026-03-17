include $(TOPDIR)/rules.mk

PKG_NAME:=uwe5622
PKG_RELEASE:=1

PKG_BUILD_PARALLEL:=1
PKG_FORTIFY_SOURCE:=0

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/uwe5622
  SUBMENU:=Wireless Drivers
  TITLE:=Unisoc UWE5622 Support
  DEPENDS:= +kmod-cfg80211 +kmod-crypto-sha256\
	+@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT\
        +uwe5622-firmware
  FILES:= \
	$(PKG_BUILD_DIR)/uwe5622_bsp_sdio.ko \
	$(PKG_BUILD_DIR)/sprdwl_ng.ko
  AUTOLOAD:=$(call AutoProbe,uwe5622_bsp_sdio sprdwl_ng)
endef

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(PKG_BUILD_DIR)/unisocwcn/include \
	-include backport/backport.h

NOSTDINC_FLAGS+=-DBUILD_OPENWRT

define Build/Compile
	+$(KERNEL_MAKE) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

$(eval $(call KernelPackage,uwe5622))

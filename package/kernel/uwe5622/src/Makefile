ifneq ($(KERNELRELEASE),)

# -----------------------------------------------------------------------------
# Kbuild section
# -----------------------------------------------------------------------------

# Default board selection; can be overridden on the make command line.
CONFIG_AW_WIFI_DEVICE_UWE5622 ?= y

# Module names
MODULE_NAME1 := uwe5622_bsp_sdio
ifeq ($(strip $(UNISOC_MODULE_NAME)),)
MODULE_NAME := sprdwl_ng
else
MODULE_NAME := $(UNISOC_MODULE_NAME)
endif

# Common compiler flags
ccflags-y += -fno-pic
ccflags-y += -D__linux__

# Common include paths
ccflags-y += -I$(src)/unisocwcn/include/
ccflags-y += -I$(src)/unisocwcn/platform/
ccflags-y += -I$(src)/unisocwcn/platform/rf/

# -----------------------------------------------------------------------------
# unisocwcn / BSP configuration
# -----------------------------------------------------------------------------

ifeq ($(CONFIG_AW_WIFI_DEVICE_UWE5622),y)
CONFIG_WCN_SDIO := y
CONFIG_WCN_SWD  := y
BSP_CHIP_ID     := uwe5622
WCN_HW_TYPE     := sdio

ccflags-y += -DCONFIG_CHECK_DRIVER_BY_CHIPID
ccflags-y += -DCONFIG_AW_BOARD

ifneq ($(TARGET_BUILD_VARIANT),user)
ccflags-y += -DCONFIG_WCN_LOOPCHECK
endif
endif

ccflags-y += -DCONFIG_CUSTOMIZE_UNISOC_FW_PATH=\"/lib/firmware\"

ifneq ($(strip $(UNISOC_DBG_FILESIZE_CONFIG)),)
ccflags-y += -DCONFIG_CUSTOMIZE_UNISOC_DBG_FILESIZE=$(UNISOC_DBG_FILESIZE_CONFIG)
endif

ifneq ($(strip $(UNISOC_DBG_FILENUM_CONFIG)),)
ccflags-y += -DCONFIG_CUSTOMIZE_UNISOC_DBG_FILENUM=$(UNISOC_DBG_FILENUM_CONFIG)
endif

ifneq ($(strip $(UNISOC_DBG_PATH_CONFIG)),)
ccflags-y += -DCONFIG_CUSTOMIZE_UNISOC_DBG_PATH=\"$(UNISOC_DBG_PATH_CONFIG)\"
endif

ifneq ($(strip $(UNISOC_SDIO_IRQ_TYPE_CONFIG)),)
ccflags-y += -DCONFIG_CUSTOMIZE_SDIO_IRQ_TYPE=$(UNISOC_SDIO_IRQ_TYPE_CONFIG)
endif

ifeq ($(CONFIG_WCN_SDIO),y)
ccflags-y += -DCONFIG_WCN_SDIO
ccflags-y += -DCONFIG_WCN_SLP
endif

ifeq ($(CONFIG_WCN_SWD),y)
ccflags-y += -DCONFIG_WCN_SWD
endif

ccflags-y += -DCONFIG_WCN_BOOT
ccflags-y += -DCONFIG_WCN_UTILS

# -----------------------------------------------------------------------------
# unisocwifi configuration
# -----------------------------------------------------------------------------

ifneq ($(strip $(UNISOC_BSP_INCLUDE)),)
ccflags-y += -I$(UNISOC_BSP_INCLUDE)
endif

ifneq ($(strip $(UNISOC_WIFI_CUS_CONFIG)),)
ccflags-y += -DCUSTOMIZE_WIFI_CFG_PATH=\"$(UNISOC_WIFI_CUS_CONFIG)\"
endif

ifneq ($(strip $(UNISOC_WIFI_MAC_FILE)),)
ccflags-y += -DCUSTOMIZE_WIFI_MAC_FILE=\"$(UNISOC_WIFI_MAC_FILE)\"
endif
ccflags-y += -DCONFIG_AW_BOARD
#ccflags-y += -DCONFIG_WCN_PARSE_DTS
#ccflags-y += -DCONFIG_WCN_CHIPEN
#ccflags-y += -DCONFIG_WCN_RESET
#ccflags-y += -DCONFIG_WCN_RESET_PIN_CONNECTED
ccflags-y += -DCONFIG_WCN_POWER_UP_DOWN
#ccflags-y += -DCONFIG_WCN_DOWNLOAD_FIRMWARE_FROM_HEX
ccflags-y += -DCONFIG_BT_WAKE_HOST_EN
ccflags-y += -DCONFIG_WL_WAKE_HOST_EN
#ccflags-y += -DCONFIG_WCN_RESUME_POWER_DOWN
#ccflags-y += -DCONFIG_WCN_RESUME_KEEPPWR_RESETSDIO
#ccflags-y += -DCONFIG_WCN_CHECK_MODULE_VENDOR
#ifneq ($(TARGET_BUILD_VARIANT),user)
ccflags-y += -DCONFIG_WCN_LOOPCHECK
#ccflags-y += -DCONFIG_CPLOG_DEBUG
#endif
#ccflags-y += -DCONFIG_MEM_PD
ccflags-y += -DCONFIG_SDIO_TX_ADMA_MODE
ccflags-y += -DCONFIG_SDIO_RX_ADMA_MODE
ccflags-y += -DCONFIG_SDIO_INBAND_INT
#ccflags-y += -DCONFIG_SDIO_INBAND_POLLING
#ccflags-y += -DCONFIG_SDIO_BLKSIZE_512
#ccflags-y += -DCONFIG_SDIO_PWRSEQ
ccflags-y += -DUWE5621_FTR
ccflags-y += -DIBSS_SUPPORT
ccflags-y += -DIBSS_RSN_SUPPORT
ccflags-y += -DNAN_SUPPORT
ccflags-y += -DRTT_SUPPORT
ccflags-y += -DACS_SUPPORT
ccflags-y += -DRX_HW_CSUM
ccflags-y += -DWMMAC_WFA_CERTIFICATION
ccflags-y += -DCOMPAT_SAMPILE_CODE
ccflags-y += -DRND_MAC_SUPPORT
ccflags-y += -DATCMD_ASSERT
ccflags-y += -DTCPACK_DELAY_SUPPORT
ccflags-y += -DSPLIT_STACK
ccflags-y += -DOTT_UWE
ccflags-y += -DPPPOE_LLC_SUPPORT
ccflags-y += -DSYNC_DISCONNECT

ifneq ($(TARGET_BUILD_VARIANT),user)
ccflags-y += -DWL_CONFIG_DEBUG
endif

ifeq ($(UNISOC_STA_SOFTAP_SCC_MODE),y)
ccflags-y += -DSTA_SOFTAP_SCC_MODE
endif

ccflags-$(CONFIG_UNISOC_WIFI_PS) += -DUNISOC_WIFI_PS

# -----------------------------------------------------------------------------
# Build two modules from one kbuild file
# -----------------------------------------------------------------------------

obj-m += $(MODULE_NAME1).o
obj-m += $(MODULE_NAME).o

# -----------------------------------------------------------------------------
# BSP module object list
# -----------------------------------------------------------------------------

$(MODULE_NAME1)-y += \
	unisocwcn/wcn_bus.o \
	unisocwcn/platform/wcn_boot.o \
	unisocwcn/platform/wcn_dump.o \
	unisocwcn/platform/bufring.o \
	unisocwcn/platform/wcn_txrx.o \
	unisocwcn/platform/wcn_op.o \
	unisocwcn/platform/wcn_parn_parser.o \
	unisocwcn/platform/rdc_debug.o \
	unisocwcn/platform/wcn_log.o \
	unisocwcn/platform/wcn_misc.o \
	unisocwcn/platform/wcn_procfs.o \
	unisocwcn/platform/loopcheck.o

$(MODULE_NAME1)-$(CONFIG_WCN_SWD) += \
	unisocwcn/platform/wcn_swd_dp.o

$(MODULE_NAME1)-$(CONFIG_WCN_SDIO) += \
	unisocwcn/sdio/sdiohal_main.o \
	unisocwcn/sdio/sdiohal_common.o \
	unisocwcn/sdio/sdiohal_tx.o \
	unisocwcn/sdio/sdiohal_rx.o \
	unisocwcn/sdio/sdio_v3.o \
	unisocwcn/sdio/sdiohal_ctl.o \
	unisocwcn/sleep/sdio_int.o \
	unisocwcn/sleep/slp_mgr.o \
	unisocwcn/sleep/slp_sdio.o \
	unisocwcn/sleep/slp_test.o \
	unisocwcn/platform/mem_pd_mgr.o

# -----------------------------------------------------------------------------
# Wi-Fi module object list
# -----------------------------------------------------------------------------

$(MODULE_NAME)-y += \
	unisocwifi/main.o \
	unisocwifi/cfg80211.o \
	unisocwifi/txrx.o \
	unisocwifi/cmdevt.o \
	unisocwifi/npi.o \
	unisocwifi/msg.o \
	unisocwifi/work.o \
	unisocwifi/vendor.o \
	unisocwifi/tcp_ack.o \
	unisocwifi/mm.o \
	unisocwifi/reorder.o \
	unisocwifi/wl_core.o \
	unisocwifi/tx_msg.o \
	unisocwifi/rx_msg.o \
	unisocwifi/wl_intf.o \
	unisocwifi/qos.o \
	unisocwifi/dbg_ini_util.o \
	unisocwifi/reg_domain.o \
	unisocwifi/defrag.o \
	unisocwifi/ibss.o \
	unisocwifi/nan.o \
	unisocwifi/tracer.o \
	unisocwifi/rf_marlin3.o \
	unisocwifi/rtt.o \
	unisocwifi/api_version.o \
	unisocwifi/rnd_mac_addr.o \
	unisocwifi/debug.o

else

# -----------------------------------------------------------------------------
# External module wrapper
# -----------------------------------------------------------------------------

KDIR ?= /lib/modules/$(shell uname -r)/build
PWD  := $(shell pwd)

default: modules

modules:
	$(MAKE) -C $(KDIR) M=$(PWD) modules

modules_install:
	$(MAKE) -C $(KDIR) M=$(PWD) modules_install

clean:
	$(MAKE) -C $(KDIR) M=$(PWD) clean

.PHONY: default modules modules_install clean

endif

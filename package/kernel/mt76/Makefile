include $(TOPDIR)/rules.mk

PKG_NAME:=mt76
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_VERSION:=20250719
PKG_SOURCE_URL:=https://codeload.github.com/ptpt52/mt76/tar.gz/$(PKG_SOURCE_VERSION)?
PKG_HASH:=b2226920b1a81a59f16cc4f31beff2580b68357dbebed6d1980d4d9d2ea5e564
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.gz
PKG_VERSION:=$(PKG_SOURCE_VERSION)

PKG_MAINTAINER:=Felix Fietkau <nbd@nbd.name>
PKG_USE_NINJA:=0
PKG_BUILD_PARALLEL:=1

PKG_CONFIG_DEPENDS += \
	CONFIG_PACKAGE_kmod-mt76-usb \
	CONFIG_PACKAGE_kmod-mt76x02-common \
	CONFIG_PACKAGE_kmod-mt76x0-common \
	CONFIG_PACKAGE_kmod-mt76x0u \
	CONFIG_PACKAGE_kmod-mt76x2-common \
	CONFIG_PACKAGE_kmod-mt76x2 \
	CONFIG_PACKAGE_kmod-mt76x2u \
	CONFIG_PACKAGE_kmod-mt7603 \
	CONFIG_PACKAGE_CFG80211_TESTMODE

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

CMAKE_SOURCE_DIR:=$(PKG_BUILD_DIR)/tools
CMAKE_BINARY_DIR:=$(PKG_BUILD_DIR)/tools

define KernelPackage/mt76-default
  SUBMENU:=Wireless Drivers
  DEPENDS:= \
	+kmod-mac80211 \
	+@DRIVER_11AC_SUPPORT \
	+@KERNEL_PAGE_POOL \
	@!TARGET_uml
endef

define KernelPackage/mt76
  SUBMENU:=Wireless Drivers
  TITLE:=MediaTek MT76x2/MT7603 wireless driver (metapackage)
  DEPENDS:= \
	+kmod-mt76-core +kmod-mt76x2 +kmod-mt7603
endef

define KernelPackage/mt76-core
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76xx wireless driver
  HIDDEN:=1
  FILES:=\
	$(PKG_BUILD_DIR)/mt76.ko
endef

define KernelPackage/mt76-usb
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76xx wireless driver USB support
  DEPENDS += +kmod-usb-core +kmod-mt76-core
  HIDDEN:=1
  FILES:=\
	$(PKG_BUILD_DIR)/mt76-usb.ko
endef

define KernelPackage/mt76x02-usb
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76x0/MT76x2 USB wireless driver common code
  DEPENDS+=+kmod-mt76-usb +kmod-mt76x02-common
  HIDDEN:=1
  FILES:=$(PKG_BUILD_DIR)/mt76x02-usb.ko
endef

define KernelPackage/mt76x02-common
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76x0/MT76x2 wireless driver common code
  DEPENDS+=+kmod-mt76-core
  HIDDEN:=1
  FILES:=$(PKG_BUILD_DIR)/mt76x02-lib.ko
endef

define KernelPackage/mt76x0-common
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76x0 wireless driver common code
  DEPENDS+=+kmod-mt76x02-common
  HIDDEN:=1
  FILES:=$(PKG_BUILD_DIR)/mt76x0/mt76x0-common.ko
endef

define KernelPackage/mt76x0e
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76x0E wireless driver
  DEPENDS+=@PCI_SUPPORT +kmod-mt76x0-common
  FILES:=\
	$(PKG_BUILD_DIR)/mt76x0/mt76x0e.ko
  AUTOLOAD:=$(call AutoProbe,mt76x0e)
endef

define KernelPackage/mt76x0u
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76x0U wireless driver
  DEPENDS+=+kmod-mt76x0-common +kmod-mt76x02-usb
  FILES:=\
	$(PKG_BUILD_DIR)/mt76x0/mt76x0u.ko
  AUTOLOAD:=$(call AutoProbe,mt76x0u)
endef

define KernelPackage/mt76x2-common
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76x2 wireless driver common code
  DEPENDS+=+kmod-mt76-core +kmod-mt76x02-common
  HIDDEN:=1
  FILES:=$(PKG_BUILD_DIR)/mt76x2/mt76x2-common.ko
endef

define KernelPackage/mt76x2u
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76x2U wireless driver
  DEPENDS+=+kmod-mt76x2-common +kmod-mt76x02-usb
  FILES:=\
	$(PKG_BUILD_DIR)/mt76x2/mt76x2u.ko
  AUTOLOAD:=$(call AutoProbe,mt76x2u)
endef

define KernelPackage/mt76x2
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT76x2 wireless driver
  DEPENDS+=@PCI_SUPPORT +kmod-mt76x2-common
  FILES:=\
	$(PKG_BUILD_DIR)/mt76x2/mt76x2e.ko
  AUTOLOAD:=$(call AutoProbe,mt76x2e)
endef

define KernelPackage/mt7603
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7603 wireless driver
  DEPENDS+=@PCI_SUPPORT +kmod-mt76-core
  FILES:=\
	$(PKG_BUILD_DIR)/mt7603/mt7603e.ko
  AUTOLOAD:=$(call AutoProbe,mt7603e)
endef

define KernelPackage/mt76-connac
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7615/MT79xx wireless driver common code
  HIDDEN:=1
  DEPENDS+=+kmod-mt76-core
  FILES:= $(PKG_BUILD_DIR)/mt76-connac-lib.ko
endef

define KernelPackage/mt76-sdio
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7615/MT79xx SDIO driver common code
  HIDDEN:=1
  DEPENDS+=+kmod-mt76-core +kmod-mmc
  FILES:= $(PKG_BUILD_DIR)/mt76-sdio.ko
endef

define KernelPackage/mt7615-common
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7615 wireless driver common code
  HIDDEN:=1
  DEPENDS+=@PCI_SUPPORT +kmod-mt76-core +kmod-mt76-connac +kmod-hwmon-core
  FILES:= $(PKG_BUILD_DIR)/mt7615/mt7615-common.ko
endef

define KernelPackage/mt7615-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7615e firmware
  DEPENDS+=+kmod-mt7615e
endef

define KernelPackage/mt7615e
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7615e wireless driver
  DEPENDS+=@PCI_SUPPORT +kmod-mt7615-common
  FILES:= $(PKG_BUILD_DIR)/mt7615/mt7615e.ko
  AUTOLOAD:=$(call AutoProbe,mt7615e)
endef

define KernelPackage/mt7622-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7622 firmware
  DEPENDS+=+kmod-mt7615e
endef

define KernelPackage/mt7663-firmware-ap
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7663e firmware (optimized for AP)
endef

define KernelPackage/mt7663-firmware-sta
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7663e firmware (client mode offload)
endef

define KernelPackage/mt7663-usb-sdio
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7663 USB/SDIO shared code
  DEPENDS+=+kmod-mt7615-common
  HIDDEN:=1
  FILES:= \
	$(PKG_BUILD_DIR)/mt7615/mt7663-usb-sdio-common.ko
endef

define KernelPackage/mt7663s
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7663s wireless driver
  DEPENDS+=+kmod-mt76-sdio +kmod-mt7615-common +kmod-mt7663-usb-sdio
  FILES:= \
	$(PKG_BUILD_DIR)/mt7615/mt7663s.ko
  AUTOLOAD:=$(call AutoProbe,mt7663s)
endef

define KernelPackage/mt7663u
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7663u wireless driver
  DEPENDS+=+kmod-mt76-usb +kmod-mt7615-common +kmod-mt7663-usb-sdio
  FILES:= $(PKG_BUILD_DIR)/mt7615/mt7663u.ko
  AUTOLOAD:=$(call AutoProbe,mt7663u)
endef

define KernelPackage/mt7915-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7915 firmware
  DEPENDS+=+kmod-mt7915e
endef

define KernelPackage/mt7915e
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7915e wireless driver
  DEPENDS+=@PCI_SUPPORT +kmod-mt76-connac +kmod-hwmon-core +kmod-thermal +@DRIVER_11AX_SUPPORT +@KERNEL_RELAY
  FILES:= $(PKG_BUILD_DIR)/mt7915/mt7915e.ko
  AUTOLOAD:=$(call AutoProbe,mt7915e)
endef

define KernelPackage/mt7916-firmware
  $(KernelPackage/mt76-default)
  DEPENDS+=+kmod-mt7915e
  TITLE:=MediaTek MT7916 firmware
endef

define KernelPackage/mt7981-firmware
  $(KernelPackage/mt76-default)
  DEPENDS:=@TARGET_mediatek_filogic
  TITLE:=MediaTek MT7981 firmware
endef

define KernelPackage/mt7986-firmware
  $(KernelPackage/mt76-default)
  DEPENDS:=@TARGET_mediatek_filogic
  TITLE:=MediaTek MT7986 firmware
endef

define KernelPackage/mt7921-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7921 firmware
endef

define KernelPackage/mt7922-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7922 firmware
endef

define KernelPackage/mt792x-common
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT792x wireless driver common code
  HIDDEN:=1
  DEPENDS+=+kmod-mt76-connac +@DRIVER_11AX_SUPPORT
  FILES:= $(PKG_BUILD_DIR)/mt792x-lib.ko
endef

define KernelPackage/mt792x-usb
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT792x wireless driver USB code
  HIDDEN:=1
  DEPENDS+=+kmod-mt792x-common +kmod-mt76-usb +@DRIVER_11AX_SUPPORT
  FILES:= $(PKG_BUILD_DIR)/mt792x-usb.ko
endef

define KernelPackage/mt7921-common
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7921 wireless driver common code
  HIDDEN:=1
  DEPENDS+=+kmod-mt792x-common +kmod-mt7921-firmware +@DRIVER_11AX_SUPPORT +kmod-hwmon-core
  FILES:= $(PKG_BUILD_DIR)/mt7921/mt7921-common.ko
endef

define KernelPackage/mt7921u
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7921U wireless driver
  DEPENDS+=+kmod-mt792x-usb +kmod-mt7921-common
  FILES:= $(PKG_BUILD_DIR)/mt7921/mt7921u.ko
  AUTOLOAD:=$(call AutoProbe,mt7921u)
endef

define KernelPackage/mt7921s
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7921S wireless driver
  DEPENDS+=+kmod-mt76-sdio +kmod-mt7921-common
  FILES:= $(PKG_BUILD_DIR)/mt7921/mt7921s.ko
  AUTOLOAD:=$(call AutoProbe,mt7921s)
endef

define KernelPackage/mt7921e
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7921e wireless driver
  DEPENDS+=@PCI_SUPPORT +kmod-mt7921-common
  FILES:= $(PKG_BUILD_DIR)/mt7921/mt7921e.ko
  AUTOLOAD:=$(call AutoProbe,mt7921e)
endef

define KernelPackage/mt7996e
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7996E wireless driver
  DEPENDS+=@PCI_SUPPORT +kmod-mt76-connac +kmod-hwmon-core +@DRIVER_11AX_SUPPORT \
  	   +@KERNEL_RELAY +@DRIVER_11BE_SUPPORT
  FILES:= $(PKG_BUILD_DIR)/mt7996/mt7996e.ko
  AUTOLOAD:=$(call AutoProbe,mt7996e)
endef

define KernelPackage/mt7992-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7992 firmware
  DEPENDS+=+kmod-mt7996e
endef

define KernelPackage/mt7992-23-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7996 firmware (2+3 antenna variant)
  DEPENDS+=+kmod-mt7996e
endef

define KernelPackage/mt7996-firmware-common
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7996 firmware (common files)
  HIDDEN:=1
endef

define KernelPackage/mt7996-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7996 firmware
  DEPENDS+=+kmod-mt7996e +kmod-mt7996-firmware-common
endef

define KernelPackage/mt7996-233-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7996 firmware (2+3+3 antenna variant)
  DEPENDS+=+kmod-mt7996e +kmod-mt7996-firmware-common
endef

define KernelPackage/mt7925-firmware
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7925 firmware
  DEPENDS+=+kmod-mt7925e
endef

define KernelPackage/mt7925-common
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7925 wireless driver common code
  HIDDEN:=1
  DEPENDS+=+kmod-mt792x-common +@DRIVER_11AX_SUPPORT +kmod-hwmon-core +@DRIVER_11BE_SUPPORT
  FILES:= $(PKG_BUILD_DIR)/mt7925/mt7925-common.ko
endef

define KernelPackage/mt7925u
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7925U wireless driver
  DEPENDS+=+kmod-mt792x-usb +kmod-mt7925-common
  FILES:= $(PKG_BUILD_DIR)/mt7925/mt7925u.ko
  AUTOLOAD:=$(call AutoProbe,mt7925u)
endef

define KernelPackage/mt7925e
  $(KernelPackage/mt76-default)
  TITLE:=MediaTek MT7925e wireless driver
  DEPENDS+=@PCI_SUPPORT +kmod-mt7925-common
  FILES:= $(PKG_BUILD_DIR)/mt7925/mt7925e.ko
  AUTOLOAD:=$(call AutoProbe,mt7925e)
endef

define Package/mt76-test
  SECTION:=devel
  CATEGORY:=Development
  TITLE:=mt76 testmode CLI
  DEPENDS:=kmod-mt76-core +libnl-tiny
endef

TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include/libnl-tiny

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/autoconf.h \
	-include backport/backport.h

ifdef CONFIG_PACKAGE_MAC80211_MESH
  NOSTDINC_FLAGS += -DCONFIG_MAC80211_MESH
endif

ifdef CONFIG_PACKAGE_CFG80211_TESTMODE
  NOSTDINC_FLAGS += -DCONFIG_NL80211_TESTMODE
  PKG_MAKE_FLAGS += CONFIG_NL80211_TESTMODE=y
endif

ifdef CONFIG_PACKAGE_kmod-mt76-usb
  PKG_MAKE_FLAGS += CONFIG_MT76_USB=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76x02-common
  PKG_MAKE_FLAGS += CONFIG_MT76x02_LIB=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76x02-usb
  PKG_MAKE_FLAGS += CONFIG_MT76x02_USB=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76x0-common
  PKG_MAKE_FLAGS += CONFIG_MT76x0_COMMON=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76x0e
  PKG_MAKE_FLAGS += CONFIG_MT76x0E=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76x0u
  PKG_MAKE_FLAGS += CONFIG_MT76x0U=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76x2-common
  PKG_MAKE_FLAGS += CONFIG_MT76x2_COMMON=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76x2
  PKG_MAKE_FLAGS += CONFIG_MT76x2E=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76x2u
  PKG_MAKE_FLAGS += CONFIG_MT76x2U=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7603
  PKG_MAKE_FLAGS += CONFIG_MT7603E=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76-connac
  PKG_MAKE_FLAGS += CONFIG_MT76_CONNAC_LIB=m
endif
ifdef CONFIG_PACKAGE_kmod-mt76-sdio
  PKG_MAKE_FLAGS += CONFIG_MT76_SDIO=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7615-common
  PKG_MAKE_FLAGS += CONFIG_MT7615_COMMON=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7615e
  PKG_MAKE_FLAGS += CONFIG_MT7615E=m
  ifdef CONFIG_TARGET_mediatek_mt7622
    PKG_MAKE_FLAGS += CONFIG_MT7622_WMAC=y
    NOSTDINC_FLAGS += -DCONFIG_MT7622_WMAC
  endif
endif
ifdef CONFIG_PACKAGE_kmod-mt7663-usb-sdio
  PKG_MAKE_FLAGS += CONFIG_MT7663_USB_SDIO_COMMON=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7663s
  PKG_MAKE_FLAGS += CONFIG_MT7663S=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7663u
  PKG_MAKE_FLAGS += CONFIG_MT7663U=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7915e
  PKG_MAKE_FLAGS += CONFIG_MT7915E=m
  ifdef CONFIG_TARGET_mediatek_filogic
    PKG_MAKE_FLAGS += CONFIG_MT798X_WMAC=y
    NOSTDINC_FLAGS += -DCONFIG_MT798X_WMAC
  endif
endif
ifdef CONFIG_PACKAGE_kmod-mt792x-common
  PKG_MAKE_FLAGS += CONFIG_MT792x_LIB=m
endif
ifdef CONFIG_PACKAGE_kmod-mt792x-usb
  PKG_MAKE_FLAGS += CONFIG_MT792x_USB=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7921-common
  PKG_MAKE_FLAGS += CONFIG_MT7921_COMMON=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7921u
  PKG_MAKE_FLAGS += CONFIG_MT7921U=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7921s
  PKG_MAKE_FLAGS += CONFIG_MT7921S=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7921e
  PKG_MAKE_FLAGS += CONFIG_MT7921E=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7996e
  PKG_MAKE_FLAGS += CONFIG_MT7996E=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7925-common
  PKG_MAKE_FLAGS += CONFIG_MT7925_COMMON=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7925u
  PKG_MAKE_FLAGS += CONFIG_MT7925U=m
endif
ifdef CONFIG_PACKAGE_kmod-mt7925e
  PKG_MAKE_FLAGS += CONFIG_MT7925E=m
endif

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		$(PKG_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
	$(MAKE) -C $(PKG_BUILD_DIR)/tools
endef

define Build/Install
	:
endef

define Package/kmod-mt76/install
	true
endef

define KernelPackage/mt76x0-common/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7610e.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt76x2-common/install
	$(INSTALL_DIR) $(1)/lib/firmware
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7662_rom_patch.bin \
		$(PKG_BUILD_DIR)/firmware/mt7662.bin \
		$(1)/lib/firmware
endef

define KernelPackage/mt76x0u/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	ln -sf mt7610e.bin $(1)/lib/firmware/mediatek/mt7610u.bin
endef

define KernelPackage/mt76x2u/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	ln -sf ../mt7662.bin $(1)/lib/firmware/mediatek/mt7662u.bin
	ln -sf ../mt7662_rom_patch.bin $(1)/lib/firmware/mediatek/mt7662u_rom_patch.bin
endef

define KernelPackage/mt7603/install
	$(INSTALL_DIR) $(1)/lib/firmware
	cp $(if $(CONFIG_TARGET_ramips_mt76x8), \
		$(PKG_BUILD_DIR)/firmware/mt7628_e1.bin \
		$(PKG_BUILD_DIR)/firmware/mt7628_e2.bin \
		,\
		$(PKG_BUILD_DIR)/firmware/mt7603_e1.bin \
		$(PKG_BUILD_DIR)/firmware/mt7603_e2.bin \
		) \
		$(1)/lib/firmware
endef

define KernelPackage/mt7615-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7615_cr4.bin \
		$(PKG_BUILD_DIR)/firmware/mt7615_n9.bin \
		$(PKG_BUILD_DIR)/firmware/mt7615_rom_patch.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7622-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7622_n9.bin \
		$(PKG_BUILD_DIR)/firmware/mt7622_rom_patch.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7663-firmware-ap/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7663_n9_rebb.bin \
		$(PKG_BUILD_DIR)/firmware/mt7663pr2h_rebb.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7663-firmware-sta/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7663_n9_v3.bin \
		$(PKG_BUILD_DIR)/firmware/mt7663pr2h.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7915-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7915_wa.bin \
		$(PKG_BUILD_DIR)/firmware/mt7915_wm.bin \
		$(PKG_BUILD_DIR)/firmware/mt7915_rom_patch.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7916-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7916_wa.bin \
		$(PKG_BUILD_DIR)/firmware/mt7916_wm.bin \
		$(PKG_BUILD_DIR)/firmware/mt7916_rom_patch.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7981-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7981_wa.bin \
		$(PKG_BUILD_DIR)/firmware/mt7981_wm.bin \
		$(PKG_BUILD_DIR)/firmware/mt7981_rom_patch.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7986-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7986_wa.bin \
		$(PKG_BUILD_DIR)/firmware/mt7986_wm_mt7975.bin \
		$(PKG_BUILD_DIR)/firmware/mt7986_wm.bin \
		$(PKG_BUILD_DIR)/firmware/mt7986_rom_patch_mt7975.bin \
		$(PKG_BUILD_DIR)/firmware/mt7986_rom_patch.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7921-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/WIFI_MT7961_patch_mcu_1_2_hdr.bin \
		$(PKG_BUILD_DIR)/firmware/WIFI_RAM_CODE_MT7961_1.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7922-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek
	cp \
		$(PKG_BUILD_DIR)/firmware/WIFI_MT7922_patch_mcu_1_1_hdr.bin \
		$(PKG_BUILD_DIR)/firmware/WIFI_RAM_CODE_MT7922_1.bin \
		$(1)/lib/firmware/mediatek
endef

define KernelPackage/mt7925-firmware/install
        $(INSTALL_DIR) $(1)/lib/firmware/mediatek/mt7925
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7925/WIFI_MT7925_PATCH_MCU_1_1_hdr.bin \
		$(PKG_BUILD_DIR)/firmware/mt7925/WIFI_RAM_CODE_MT7925_1_1.bin \
		$(1)/lib/firmware/mediatek/mt7925
endef

define KernelPackage/mt7992-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek/mt7996
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_dsp.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_eeprom.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_eeprom_2i5i.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_eeprom_2i5e.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_rom_patch.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_wa.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_wm.bin \
		$(1)/lib/firmware/mediatek/mt7996
endef

define KernelPackage/mt7992-23-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek/mt7996
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_dsp_23.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_eeprom_23.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_eeprom_23_2i5i.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_rom_patch_23.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_wa_23.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7992_wm_23.bin \
		$(1)/lib/firmware/mediatek/mt7996
endef

define KernelPackage/mt7996-firmware-common/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek/mt7996
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_dsp.bin \
		$(1)/lib/firmware/mediatek/mt7996
endef

define KernelPackage/mt7996-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek/mt7996
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_eeprom.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_eeprom_2i5i6i.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_rom_patch.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_wa.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_wm.bin \
		$(1)/lib/firmware/mediatek/mt7996
endef

define KernelPackage/mt7996-233-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/mediatek/mt7996
	cp \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_eeprom_233.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_eeprom_233_2i5i6i.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_rom_patch_233.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_wa_233.bin \
		$(PKG_BUILD_DIR)/firmware/mt7996/mt7996_wm_233.bin \
		$(1)/lib/firmware/mediatek/mt7996
endef

define Package/mt76-test/install
	mkdir -p $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tools/mt76-test $(1)/usr/sbin
endef

define Build/InstallDev
	mkdir -p $(STAGING_DIR_IMAGE)
	$(CP) $(PKG_BUILD_DIR)/firmware/mt7981_eeprom_mt7976_dbdc.bin $(STAGING_DIR_IMAGE)/
endef

$(eval $(call KernelPackage,mt76-core))
$(eval $(call KernelPackage,mt76-usb))
$(eval $(call KernelPackage,mt76x02-usb))
$(eval $(call KernelPackage,mt76x02-common))
$(eval $(call KernelPackage,mt76x0-common))
$(eval $(call KernelPackage,mt76x0e))
$(eval $(call KernelPackage,mt76x0u))
$(eval $(call KernelPackage,mt76x2-common))
$(eval $(call KernelPackage,mt76x2u))
$(eval $(call KernelPackage,mt76x2))
$(eval $(call KernelPackage,mt7603))
$(eval $(call KernelPackage,mt76-connac))
$(eval $(call KernelPackage,mt76-sdio))
$(eval $(call KernelPackage,mt7615-common))
$(eval $(call KernelPackage,mt7615-firmware))
$(eval $(call KernelPackage,mt7622-firmware))
$(eval $(call KernelPackage,mt7615e))
$(eval $(call KernelPackage,mt7663-firmware-ap))
$(eval $(call KernelPackage,mt7663-firmware-sta))
$(eval $(call KernelPackage,mt7663-usb-sdio))
$(eval $(call KernelPackage,mt7663u))
$(eval $(call KernelPackage,mt7663s))
$(eval $(call KernelPackage,mt7915-firmware))
$(eval $(call KernelPackage,mt7915e))
$(eval $(call KernelPackage,mt7916-firmware))
$(eval $(call KernelPackage,mt7981-firmware))
$(eval $(call KernelPackage,mt7986-firmware))
$(eval $(call KernelPackage,mt7921-firmware))
$(eval $(call KernelPackage,mt7922-firmware))
$(eval $(call KernelPackage,mt7925-firmware))
$(eval $(call KernelPackage,mt792x-common))
$(eval $(call KernelPackage,mt792x-usb))
$(eval $(call KernelPackage,mt7921-common))
$(eval $(call KernelPackage,mt7925-common))
$(eval $(call KernelPackage,mt7921u))
$(eval $(call KernelPackage,mt7921s))
$(eval $(call KernelPackage,mt7921e))
$(eval $(call KernelPackage,mt7925u))
$(eval $(call KernelPackage,mt7925e))
$(eval $(call KernelPackage,mt7996e))
$(eval $(call KernelPackage,mt7992-firmware))
$(eval $(call KernelPackage,mt7992-23-firmware))
$(eval $(call KernelPackage,mt7996-firmware-common))
$(eval $(call KernelPackage,mt7996-firmware))
$(eval $(call KernelPackage,mt7996-233-firmware))
$(eval $(call KernelPackage,mt76))
$(eval $(call BuildPackage,mt76-test))

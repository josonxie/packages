#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=hid
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=0.7.2
PKG_RELEASE:=3

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/node-hid/node-hid.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=v0.7.2
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MIRROR_HASH:=ede801a26a23290ab76d64ab636c3c3e2788030bb830af7006d37444c2a7b2c4

PKG_BUILD_DEPENDS:=node/host libudev-fbsd
PKG_NODE_VERSION:=8.10.0

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>
PKG_LICENSE:=Custom
PKG_LICENSE_FILES:=

include $(INCLUDE_DIR)/package.mk

define Package/node-hid
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Node.js package to access HID devices
  URL:=https://github.com/node-hid/node-hid
  DEPENDS:=+node +node-npm +libusb-1.0 +hidapi +libstdcpp
endef

define Package/node-hid/description
 Node.js package to access HID devices
endef

CPU:=$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))

TMPNPM:=$(shell mktemp -u XXXXXXXXXX)

TARGET_CFLAGS+=$(FPIC) -I$(STAGING_DIR)/usr/include/libusb-1.0

define Build/Compile
	git init $(PKG_BUILD_DIR)
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(CONFIG_ARCH) \
	npm_config_nodedir=$(BUILD_DIR)/node-v$(PKG_NODE_VERSION)/ \
	npm_config_cache=$(BUILD_DIR)/node-v$(PKG_NODE_VERSION)/npm-cache-$(TMPNPM) \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	npm install --build-from-source --target_arch=$(CPU) -g $(PKG_BUILD_DIR)
endef

define Package/node-hid/install
	mkdir -p $(1)/usr/lib/node/node-hid/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/ $(1)/usr/lib/node/
endef

$(eval $(call BuildPackage,node-hid))


#
# Copyright (C) 2019-2020 expiron <expiron18@gmail.com>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE.md for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=tsauth
PKG_VERSION:=0.3.5
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/expiron/tsauth/tar.gz/v$(PKG_VERSION)?
PKG_SOURCE_DATE:=2019-08-18
PKG_HASH:=a2949b98cb945d9dadb905d607b81d08d00c7c19d1c3297ea53b49aa1fc7ed30
CMAKE_INSTALL:=1
PKG_USE_MIPS16:=0

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE.md

PKG_MAINTAINER:=expiron <expiron18@gmail.com>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/tsauth
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+curl +libjson-c +libmbedtls
  TITLE:=Tsinghua Network Authorization Client
  URL:=https://github.com/expiron/tsauth.git
endef

define Package/tsauth/description
  A tiny client for Tsinghua network AAA (Authentication, Authorization, Accounting) system.
endef

define Package/tsauth/conffiles
/etc/config/tsauth
endef

define Package/tsauth/install
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/tsauth.config $(1)/etc/config/tsauth
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_DATA) ./files/tsauth.hotplug $(1)/etc/hotplug.d/iface/99-tsauth
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/tsauth.init $(1)/etc/init.d/tsauth
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) ./files/tsauth-keep-online $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tsauth $(1)/usr/bin
endef

$(eval $(call BuildPackage,tsauth))

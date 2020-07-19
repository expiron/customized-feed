#
# Copyright (C) 2019-2020 expiron <expiron18@gmail.com>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE.md for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=v2ray-core
PKG_VERSION:=4.26.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/v2ray/v2ray-core/tar.gz/v${PKG_VERSION}?
PKG_SOURCE_DATE:=2020-07-08
PKG_HASH:=b2c80d0f7935d33e922ecceaed980ddd1a8aa332f57f6a8b1840fae6e37e98cb

PKG_MAINTAINER:=expiron <expiron18@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=v2ray.com/core

GO_PKG_LDFLAGS:=-s -w
GO_PKG_LDFLAGS_X:=v2ray.com/core.codename=user v2ray.com/core.build=$(shell date '+%Y%m%d-%H%M%S') v2ray.com/core.version=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/v2ray-core
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=V2Ray Core
  URL:=https://www.v2ray.com
  DEPENDS:=+ca-certificates $(GO_ARCH_DEPENDS)
endef

define Package/v2ray-core/description
  Project V (V2Ray) is a set of network tools that help you to build your own computer network. It secures
your network connections and thus protects your privacy. This package includes v2ray, v2ctl and v2ray-assets.
endef

define Package/v2ray-core/conffiles
/etc/config/v2ray
/etc/v2ray/v2ray.json
endef

# As geoip.dat and geosite.dat update automatically everyday.
# It's unnecessary and boring to update their versions and hashes.
GEOIP_FILE:=geoip.$(shell curl --connect-timeout 30 --retry 5 --insecure -fsSL 'https://api.github.com/repos/v2ray/geoip/releases/latest' | jq -rM '.tag_name').dat
define Download/geoip
  URL:=https://github.com/v2ray/geoip/raw/release/
  URL_FILE:=geoip.dat
  HASH:=$(shell curl --connect-timeout 30 --retry 5 --insecure -fsSL 'https://github.com/v2ray/geoip/raw/release/geoip.dat.sha256sum' | grep -oE '[[:xdigit:]]{64}')
  FILE:=$(GEOIP_FILE)
endef
$(eval $(call Download,geoip))

GEOSITE_FILE:=geosite.$(shell curl --connect-timeout 30 --retry 5 --insecure -fsSL 'https://api.github.com/repos/v2ray/domain-list-community/releases/latest' | jq -rM '.tag_name').dat
define Download/geosite
  URL:=https://github.com/v2ray/domain-list-community/raw/release/
  URL_FILE:=dlc.dat
  HASH:=$(shell curl --connect-timeout 30 --retry 5 --insecure -fsSL 'https://github.com/v2ray/domain-list-community/raw/release/dlc.dat.sha256sum' | grep -oE '[[:xdigit:]]{64}')
  FILE:=$(GEOSITE_FILE)
endef
$(eval $(call Download,geosite))

define Build/Compile
	$(eval GO_PKG_BUILD_PKG:=$(GO_PKG)/main)
	$(call GoPackage/Build/Compile,)
	mv -vf $(GO_PKG_BUILD_BIN_DIR)/main $(GO_PKG_BUILD_BIN_DIR)/v2ray

	$(eval GO_PKG_BUILD_PKG:=$(GO_PKG)/infra/control/main)
	$(call GoPackage/Build/Compile,)
	mv -vf $(GO_PKG_BUILD_BIN_DIR)/main $(GO_PKG_BUILD_BIN_DIR)/v2ctl
endef

define Package/v2ray-core/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/v2ray.config $(1)/etc/config/v2ray
	$(INSTALL_DIR) $(1)/etc/v2ray
	$(INSTALL_CONF) ./files/v2ray.json.config $(1)/etc/v2ray/v2ray.json
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/v2ray.init $(1)/etc/init.d/v2ray
	$(INSTALL_DIR) $(1)/usr/bin/v2ray
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/v2{ray,ctl} $(1)/usr/bin/v2ray
	$(INSTALL_DATA) $(DL_DIR)/$(GEOIP_FILE) $(1)/usr/bin/v2ray
	$(LN) -s $(GEOIP_FILE) $(1)/usr/bin/v2ray/geoip.dat
	$(INSTALL_DATA) $(DL_DIR)/$(GEOSITE_FILE) $(1)/usr/bin/v2ray
	$(LN) -s $(GEOSITE_FILE) $(1)/usr/bin/v2ray/geosite.dat
endef

$(eval $(call GoBinPackage,v2ray-core))
$(eval $(call BuildPackage,v2ray-core))

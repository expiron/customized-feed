#
# Copyright (C) 2019-2020 expiron <expiron18@gmail.com>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE.md for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=kcptun
PKG_VERSION:=20200701
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/xtaci/kcptun/tar.gz/v${PKG_VERSION}?
PKG_SOURCE_DATE:=2020-07-01
PKG_HASH:=d5b2d212c6806f1c4eba5fbce8797734eaa8ae0f8cdd90dd06d0844392888ff0

PKG_MAINTAINER:=expiron <expiron18@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE.md

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=github.com/xtaci/kcptun

GO_PKG_LDFLAGS:=-s -w
GO_PKG_LDFLAGS_X:=main.VERSION=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/kcptun-config
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=Kcptun Config Scripts
  URL:=https://github.com/xtaci/kcptun
endef

define Package/kcptun-config/conffiles
/etc/config/kcptun
endef

define Package/kcptun-config/install
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/kcptun.config $(1)/etc/config/kcptun
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/kcptun.init $(1)/etc/init.d/kcptun
endef

define Package/kcptun/Default
  define Package/kcptun-$(1)
    SECTION:=net
    CATEGORY:=Network
    SUBMENU:=Web Servers/Proxies
    TITLE:=KCP-based Secure Tunnel $(1)
    URL:=https://github.com/xtaci/kcptun
    DEPENDS:=+kcptun-config $$(GO_ARCH_DEPENDS)
  endef

  define Package/kcptun-$(1)/description
    kcptun is a Stable & Secure Tunnel Based On KCP with N:M Multiplexing.
This package only contains kcptun $(1).
  endef

  define Package/kcptun-$(1)/install
		$$(call GoPackage/Package/Install/Bin,$$(PKG_INSTALL_DIR))

		$$(INSTALL_DIR) $$(1)/usr/bin
		$$(INSTALL_BIN) $$(PKG_INSTALL_DIR)/usr/bin/$(1) $$(1)/usr/bin/kcptun-$(1)
  endef
endef

$(eval $(call BuildPackage,kcptun-config))
KCPTUN_COMPONENTS:=server client
$(foreach component,$(KCPTUN_COMPONENTS), \
  $(eval $(call Package/kcptun/Default,$(component))) \
  $(eval $(call GoBinPackage,kcptun-$(component))) \
  $(eval $(call BuildPackage,kcptun-$(component))) \
)

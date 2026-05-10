# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024 Your Name <your.email@example.com>
#

include $(TOPDIR)/rules.mk

PKG_NAME:=opensoho
PKG_VERSION:=0.12.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/rubenbe/opensoho.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_MIRROR_HASH:=skip

# Go build dependencies and flags
PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

# Go module path (from go.mod: module github.com/rubenbe/opensoho)
GO_PKG:=github.com/rubenbe/opensoho
# Embed version string into the pocketbase core at link time
GO_PKG_LDFLAGS_X:=github.com/rubenbe/pocketbase.Version=$(PKG_VERSION)
# Pure Go — no CGO, no C library dependency
GO_PKG_CGO_ENABLED:=0

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/opensoho
  SECTION:=net
  CATEGORY:=Network
  TITLE:=OpenSOHO — lightweight network management daemon
  URL:=https://github.com/rubenbe/opensoho
  MAINTAINER:=Your Name <your.email@example.com>
  DEPENDS:=$(GO_ARCH_DEPENDS)
  LICENSE:=GPL-2.0-only
  LICENSE_FILES:=LICENSE
endef

define Package/opensoho/description
  OpenSOHO is a lightweight network management and monitoring system
  distributed as a single self-contained static binary.  It is designed
  as a simpler alternative to OpenWISP, built on top of PocketBase.
endef

# Declare the UCI config file as a conffile so opkg/apk preserves
# local edits across upgrades and warns before overwriting.
define Package/opensoho/conffiles
/etc/config/opensoho
endef

# golang-package.mk sets up Build/Prepare and Build/Compile via
# GoBinPackage.  All we need to provide is the install step.
define Package/opensoho/install
	$(INSTALL_DIR) $(1)/usr/sbin
	# Compiled binary placed by golang-package.mk into GO_PKG_BUILD_BIN_DIR
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/opensoho $(1)/usr/sbin/opensoho
	$(INSTALL_BIN) ./files/usr/sbin/configure-opensoho \
		$(1)/usr/sbin/configure-opensoho

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/etc/config/opensoho $(1)/etc/config/opensoho

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/opensoho $(1)/etc/init.d/opensoho
endef

# GoBinPackage wires up Build/Prepare + Build/Compile for a Go binary.
# BuildPackage is the standard OpenWrt package registration macro.
$(eval $(call GoBinPackage,opensoho))
$(eval $(call BuildPackage,opensoho))

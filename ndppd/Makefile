#
# Copyright (C) 2007-2011 BoluWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ndppd
PKG_VERSION:=0.2.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_VERSION).tar.gz

# Latest release
PKG_SOURCE_URL:=https://github.com/DanielAdolfsson/ndppd/archive
PKG_MD5SUM:=1391c063db64b47541e58da12e5ae60d
PKG_LICENSE:=GPL-3.0+

# Development snapshot
#PKG_SOURCE_URL=git://github.com/Tuhox/ndppd.git
#PKG_SOURCE_VERSION=master
#PKG_SOURCE_SUBDIR=$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/uclibc++.mk
include $(INCLUDE_DIR)/package.mk

define Package/ndppd
  SECTION:=net
  CATEGORY:=Network
  TITLE:=NDP Proxy Daemon
  URL:=http://www.priv.nu/projects/ndppd/
  MAINTAINER:=Gabriel Kerneis <gabriel@kerneis.info>
  DEPENDS:=@IPV6 $(CXX_DEPENDS)
endef

define Package/ndppd/description
 ndppd, or NDP Proxy Daemon, is a daemon that proxies NDP (Neighbor Discovery
 Protocol) messages between interfaces.  ndppd currently only supports Neighbor
 Solicitation Messages and Neighbor Advertisement Messages.

 The ndp_proxy provided by Linux doesn't support listing proxies, and only hosts
 are supported.  No subnets.  ndppd solves this by listening for Neighbor
 Solicitation messages on an interface, then query the internal interfaces for
 that target IP before finally sending a Neighbor Advertisement message.
endef

define Package/ndppd/conffiles
/etc/ndppd.conf
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CXX="$(TARGET_CXX)" \
		CXXFLAGS="$(TARGET_CXXFLAGS) -std=c++0x -fno-rtti" \
		LDFLAGS="$(TARGET_LDFLAGS)" \
		LIBS="-lc" \
		ndppd
endef

define Package/ndppd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ndppd $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/ndppd.init $(1)/etc/init.d/ndppd
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/ndppd.conf-dist $(1)/etc/ndppd.conf
endef

$(eval $(call BuildPackage,ndppd))

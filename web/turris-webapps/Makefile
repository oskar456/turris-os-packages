#
# Copyright (C) 2018-2019 CZ.NIC z.s.p.o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=turris-webapps
PKG_VERSION:=1.2
PKG_RELEASE:=3

PKG_MAINTAINER:=CZ.NIC <packaging@turris.cz>
PKG_LICENSE:=GPL-3.0-or-later

include $(INCLUDE_DIR)/package.mk

define Package/turris-webapps
	TITLE:=Turris WebApps
	DEPENDS:=+lighttpd +lighttpd-mod-alias +lighttpd-mod-cgi
endef

define Package/turris-webapps/description
 Common landding page for various installed web apps.
endef

Build/Compile:=:

define Package/turris-webapps/install
	$(INSTALL_DIR) $(1)/etc/lighttpd/conf.d/
	$(INSTALL_DATA) ./files/turris-root.conf $(1)/etc/lighttpd/conf.d/turis-root.conf
	$(INSTALL_DIR) $(1)/usr/share/turris-webapps
	$(INSTALL_BIN) ./files/turris-root-cgi $(1)/usr/share/turris-webapps
	$(INSTALL_DIR) $(1)/www/webapps-icons
	$(INSTALL_DATA) ./files/generic-web.svg $(1)/www/webapps-icons/
endef

define Package/turris-webapps/postinst
#!/bin/sh
[ -n "$$IPKG_INSTROOT" ] || {
	/etc/init.d/lighttpd restart
}
endef

$(eval $(call BuildPackage,turris-webapps))

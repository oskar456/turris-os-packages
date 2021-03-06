#
# Copyright (C) 2018-2020 CZ.NIC, z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nextcloud
PKG_VERSION:=16.0.9
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://download.nextcloud.com/server/releases/
PKG_HASH:=e135102c987f9cf5bde7e2ae055faec3ddf07252aa8c7f89fb5162472c574937

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

PKG_MAINTAINER:=CZ.NIC <packaging@turris.cz>
PKG_LICENSE:=AGPL-3.0
PKG_LICENSE_FILES:=COPYING

PKG_INSTALL:=0

include $(INCLUDE_DIR)/package.mk

define Package/nextcloud
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Nextcloud server
	URL:=https://nextcloud.com/
	DEPENDS=+php7-mod-opcache +php7-fpm +php7-cli +sudo +php7-mod-ctype +php7-mod-fileinfo +php7-mod-xmlreader +php7-mod-openssl +php7-mod-hash +php7-mod-gd +php7-mod-xml +php7-mod-xmlwriter +php7-mod-json +php7-mod-mbstring +php7-mod-pcntl +php7-mod-opcache +php7-mod-session +php7-mod-simplexml +php7-mod-iconv +php7-mod-curl +php7-mod-zip +php7-mod-dom +coreutils-stat +php7 +lighttpd-mod-access +php7-mod-intl
endef

define Package/nextcloud-install
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Nextcloud server installation script
	DEPENDS=+nextcloud +mariadb-server +php7-mod-pdo-mysql +mariadb-client
endef

define Package/$(PKG_NAME)/description
  A safe home for all your data.
endef

define Package/$(PKG_NAME)_install/description
  Nextcloud is a safe home for all your data. And this is simple installation script.
endef

define Build/Compile
endef

define Build/Install
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/srv/www/nextcloud
	for i in `ls -1 $(PKG_BUILD_DIR)`; do \
		expr $$$$i : ipkg || $(CP) -r $(PKG_BUILD_DIR)/$$$$i $(1)/srv/www/nextcloud;\
	done
	$(INSTALL_DIR) $(1)/etc/lighttpd/conf.d/
	$(INSTALL_DATA) ./files/nextcloud.conf $(1)/etc/lighttpd/conf.d/
	$(INSTALL_DIR) $(1)/usr/share/turris-webapps
	$(INSTALL_DATA) ./files/30_nextcloud.conf $(1)/usr/share/turris-webapps/
	$(INSTALL_DIR) $(1)/www/webapps-icons
	$(INSTALL_DATA) ./files/nextcloud.png $(1)/www/webapps-icons
endef

define Package/$(PKG_NAME)-install/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) ./files/easy_install.sh $(1)/usr/bin/nextcloud_install
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
# check if we are on real system
if [ -z "$$IPKG_INSTROOT" ]; then
	if [ -f /srv/www/nextcloud/config/config.php ]; then
		cd /srv/www/nextcloud/
		VER="`cat ./config/config.php | sed -n 's|.*version[^0-9]*\([0-9]\+\)\.\([0-9]\+\)\.\([0-9]\+\)\..*|\1\2\3|p'`"
		sed -i 's|\(.*dbhost.*=>.*\)localhost\(.\),|\1127.0.0.1\2,|' config/config.php
		if [ -n "$$VER" ] && [ $$VER -lt "`echo $(PKG_VERSION) | sed 's|\.||g'`" ]; then
			sudo -u nobody php-cli ./occ upgrade --no-app-disable --no-interaction
		fi
	fi
fi
chown -R nobody "$$IPKG_INSTROOT"/srv/www/nextcloud/
endef

$(eval $(call BuildPackage,nextcloud))
$(eval $(call BuildPackage,nextcloud-install))

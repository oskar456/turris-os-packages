#
## Copyright (C) 2013 CZ.NIC z.s.p.o. (http://www.nic.cz/)
#
## This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# #
#
include $(TOPDIR)/rules.mk

PKG_NAME:=lighttpd-https-cert
PKG_VERSION:=5
PKG_RELEASE:=3

PKG_MAINTAINER:=CZ.NIC <packaging@turris.cz>
PKG_LICENSE:=GPL-3.0

include $(INCLUDE_DIR)/package.mk

define Package/lighttpd-https-cert
	TITLE:=lighttpd HTTPS cert
	DEPENDS:=+lighttpd-mod-openssl
endef

Build/Compile:=:
	
define Package/lighttpd-https-cert/install
	$(INSTALL_DIR) $(1)/etc/lighttpd/conf.d/
	$(INSTALL_CONF) ./files/ssl-enable.conf $(1)/etc/lighttpd/conf.d/ssl-enable.conf
endef

define Package/lighttpd-https-cert/postinst
#!/bin/sh
if [ -z "$$IPKG_INSTROOT" ]; then
  [ -f /etc/lighttpd-self-signed.pem ] || openssl req -new -x509 -keyout /etc/lighttpd-self-signed.pem -out /etc/lighttpd-self-signed.pem -days 7300 -nodes -subj "/C=CZ/ST=Prague/L=Prague/O=Turris"
  /etc/init.d/lighttpd restart
else
  if [ \! -f /etc/lighttpd-self-signed.pem ]; then
    mkdir -p "$$IPKG_INSTROOT"/etc/uci-defaults
    echo '#!/bin/sh' > "$$IPKG_INSTROOT"/etc/uci-defaults/https-cert
    echo '[ -f /etc/lighttpd-self-signed.pem ] || openssl req -new -x509 -keyout /etc/lighttpd-self-signed.pem -out /etc/lighttpd-self-signed.pem -days 7300 -nodes -subj "/C=CZ/ST=Prague/L=Prague/O=Turris"' >> "$$IPKG_INSTROOT"/etc/uci-defaults/https-cert
  fi
fi
endef

define Package/lighttpd-https-cert/postrm
#!/bin/sh
/etc/init.d/lighttpd restart
endef

$(eval $(call BuildPackage,lighttpd-https-cert))

#
# Copyright (C) 2017-2020 CZ.NIC, z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=python-periphery
PKG_VERSION:=2.0.1
PKG_RELEASE:=1

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PYPI_NAME:=python-periphery
PKG_HASH:=5da4d5f40ff8974cf6c724587baa674d7e0593f07b6f6ee896104f11c1be18ec

include $(TOPDIR)/feeds/packages/lang/python/pypi.mk
include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-package.mk

define Package/python3-periphery
  SECTION:=lang-python
  CATEGORY:=Languages
  SUBMENU:=Python
  TITLE:=Linux Peripheral I/O
  URL:=https://github.com/vsergeev/python-periphery
  DEPENDS:=+python3-light
  VARIANT:=python3
endef

define Package/python3-periphery/description
 python-periphery is a pure Python library for GPIO, LED, PWM, SPI, I2C, MMIO, and
 Serial peripheral I/O interface access in userspace Linux.
endef

$(eval $(call Py3Package,python3-periphery))
$(eval $(call BuildPackage,python3-periphery))
$(eval $(call BuildPackage,python3-periphery-src))

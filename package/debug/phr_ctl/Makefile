#
# Copyright (C) 2023 Keenetic Limited
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=phr_ctl
PKG_VERSION:=1

include $(INCLUDE_DIR)/package.mk

define Build/Compile
endef

define Package/phr_ctl
  CATEGORY:=Debug suite
  TITLE:=NDM peripheral control and test utility
  DEPENDS:=+libc +SSP_SUPPORT:libssp +kmod-phr
  DISABLE_RSTRIP:=y
endef

define Package/phr_ctl/install
	if [ -d ./files-phr_ctl ]; then \
		$(CP) ./files-phr_ctl/* $(1); \
	fi
endef

$(eval $(call BuildPackage,phr_ctl))

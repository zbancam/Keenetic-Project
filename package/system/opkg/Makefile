#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/version.mk

PKG_NAME:=opkg
PKG_VERSION:=v0.4.4-3
PKG_LICENSE:=GPL-2.0+

PKG_SOURCE_URL:=$(NDM_STORAGE)/$(PKG_NAME)
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_SOURCE_PROTO:=git

PKG_FIXUP:=autoreconf
PKG_REMOVE_FILES = autogen.sh aclocal.m4

PKG_INSTALL:=1

HOST_BUILD_DEPENDS:=libarchive/host

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/opkg
  SECTION:=base
  CATEGORY:=Base system
  TITLE:=opkg package management system
  URL:=http://wiki.openmoko.org/wiki/Opkg
  PKGARCH:=$(or $(ARCH_PACKAGES),$(BOARD))
endef

define Package/opkg/description
  Lightweight package management system
  opkg is the opkg Package Management System, for handling
  installation and removal of packages on a system. It can
  recursively follow dependencies and download all packages
  necessary to install a particular package.

  opkg knows how to install both .ipk and .deb packages.
endef

define Package/opkg/conffiles
/etc/opkg.conf
endef

TARGET_CFLAGS += $(if $(CONFIG_GCC_VERSION_4_3)$(CONFIG_GCC_VERSION_4_4),-Wno-array-bounds)
TARGET_CFLAGS += -ffunction-sections -fdata-sections
EXTRA_CFLAGS += $(TARGET_CPPFLAGS)

CONFIGURE_ARGS += \
	--disable-curl \
	--disable-gpg \
	--disable-xz \
	--with-opkglibdir=/usr/lib \
	--with-opkgetcdir=/opt/etc \
	--with-opkglockfile=/tmp/opkg.lock

MAKE_FLAGS = \
		CC="$(TARGET_CC)" \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		HOST_CPU="$(PKGARCH)" \
		LDFLAGS="-Wl,--gc-sections" \

define Package/opkg/install
	$(INSTALL_DIR) $(1)/usr/lib/opkg
	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) ./files/opkg.conf $(1)/etc/
	$(VERSION_SED) $(1)/etc/opkg.conf
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/opkg-cl $(1)/bin/opkg
endef

define Build/InstallDev
	mkdir -p $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libopkg $(1)/usr/include/
endef


HOST_CONFIGURE_ARGS+= \
	--disable-curl \
	--disable-gpg \
	--disable-xz \
	--enable-zstd \
	--with-opkgetcdir=/etc \
	--with-opkglockfile=/tmp/opkg.lock \
	--with-static-libopkg \
	--localstatedir=/usr

define Host/Compile
	+$(MAKE) $(HOST_JOBS) -C $(HOST_BUILD_DIR) CC="$(HOSTCC)" all
endef

define Host/Install
	$(INSTALL_BIN) $(HOST_BUILD_DIR)/src/opkg $(STAGING_DIR_HOST)/bin/opkg
endef

$(eval $(call BuildPackage,opkg))
$(eval $(call HostBuild))

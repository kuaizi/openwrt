include $(TOPDIR)/rules.mk

PKG_NAME:=qBittorrent-Enhanced-Edition
PKG_VERSION:=4.2.1.10
PKG_RELEASE=2

PKG_SOURCE:=$(PKG_NAME)-release-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/c0re100/qBittorrent-Enhanced-Edition/tar.gz/release-$(PKG_VERSION)?
PKG_HASH:=78b0116c00d791328d2ad4f84f7447e2208bf0eb5798c4da2ae4404ac00cad2b

PKG_BUILD_DIR:=$(BUILD_DIR)/qBittorrent-Enhanced-Edition-release-$(PKG_VERSION)

PKG_LICENSE:=GPL-2.0+
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SUBMENU:=BitTorrent
  SECTION:=net
  CATEGORY:=Network
  TITLE:=bittorrent client programmed in C++ / Qt
  URL:=https://github.com/c0re100/qBittorrent-Enhanced-Edition
  DEPENDS:=+qt5-core +qt5-network +qt5-xml +libtorrent-rasterbar +python3
endef

define Package/$(PKG_NAME)/description
qBittorrent is a bittorrent client programmed in C++ / Qt that uses 
libtorrent (sometimes called libtorrent-rasterbar) by Arvid Norberg.
It aims to be a good alternative to all other bittorrent clients out 
there. qBittorrent is fast, stable and provides unicode support as 
well as many features.
endef

CONFIGURE_VARS += \
  	QT_QMAKE="$(TOOLCHAIN_DIR)/bin" \
	PKG_CONFIG_PATH="$(TOOLCHAIN_DIR)/lib/pkgconfig"

CONFIGURE_ARGS += \
	--disable-gui \
	--enable-stacktrace=no \
	--with-boost="$(STAGING_DIR)/usr"

MAKE_VARS += \
	INSTALL_ROOT="$(PKG_INSTALL_DIR)"

define Build/Prepare
	$(call Build/Prepare/Default)
	$(SED) '/<context>/{:a;N;/<\/context>/!ba;/\/gui\//d}' `ls $(PKG_BUILD_DIR)/src/lang/qbittorrent_*.ts`
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/man/man1
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/man/man1/qbittorrent-nox.1 $(1)/usr/man/man1/qbittorrent-nox.1
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/qbittorrent-nox $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/qBittorrent
	$(INSTALL_DATA) ./files/ipfilter.dat $(1)/etc/qBittorrent/ipfilter.dat
endef

$(eval $(call BuildPackage,$(PKG_NAME)))

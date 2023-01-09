#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=urbackup-client
PKG_VERSION_SHORT:=2.5.22
PKG_VERSION:=$(PKG_VERSION_SHORT).0
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://hndl.urbackup.org/Client/$(PKG_VERSION_SHORT)/
PKG_HASH:=e103f016e6c15b35f6abf49dadb5e34f628c1421c44cac3400ec980fe0f18da7

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/urbackup-client
  SECTION:=base
  CATEGORY:=Network
  TITLE:=UrBackup client backup system
  URL:=https://www.urbackup.org
  DEPENDS:=+zlib +libzstd +libopenssl +libstdcpp +libsqlite3
endef

define Package/urbackup-client/description
  UrBackup is an easy to setup Open Source client/server backup system,
  that through a combination of image and file backups accomplishes both
  data safety and a fast restoration time.
endef

TARGET_CFLAGS += -DCRYPTOPP_DISABLE_ASM -DCRYPTOPP_BIG_ENDIAN

CONFIGURE_ARGS:=$(filter-out	\
	--program-prefix=%	\
	--program-suffix=%	\
	--prefix=%			\
	--exec-prefix=%		\
	--bindir=%			\
	--sbindir=%			\
	--libexecdir=%		\
	--sysconfdir=%		\
	--datadir=%			\
	--localstatedir=%	\
	--mandir=%			\
	--infodir=%			\
	, $(CONFIGURE_ARGS))

CONFIGURE_ARGS+= \
	--program-prefix= \
	--sysconfdir=/etc \
	--prefix=/usr/local/urbackupclient \
	--exec-prefix=/usr \
	--with-zlib="$(STAGING_DIR)/usr" \
	--disable-clientupdate \
	--enable-fortify \
	--enable-embedded-cryptopp \
	--enable-headless \
	--without-embedded-sqlite3

define Package/urbackup-client/install
	$(INSTALL_DIR) $(1)/etc/logrotate.d
	$(INSTALL_CONF) files/logrotate_urbackupclient $(1)/etc/logrotate.d/urbackupclient
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) files/urbackupclient $(1)/etc/init.d/urbackupclient
	$(INSTALL_DIR) $(1)/etc/urbackup
	$(INSTALL_CONF) files/urbackupclient.conf $(1)/etc/urbackup/urbackupclient.conf
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/urbackup/* $(1)/etc/urbackup/
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/blockalign $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/urbackupclientctl $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/urbackupclientbackend $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/local/urbackupclient/share/urbackup
	$(CP) $(PKG_INSTALL_DIR)/usr/local/urbackupclient/share/urbackup/* $(1)/usr/local/urbackupclient/share/urbackup/
	$(INSTALL_DIR) $(1)/usr/local/urbackupclient/var/urbackup
	$(INSTALL_DIR) $(1)/usr/local/urbackupclient/var/urbackup/tmp
	$(CP) $(PKG_INSTALL_DIR)/usr/local/urbackupclient/var/urbackup/* $(1)/usr/local/urbackupclient/var/urbackup/
endef

$(eval $(call BuildPackage,urbackup-client))

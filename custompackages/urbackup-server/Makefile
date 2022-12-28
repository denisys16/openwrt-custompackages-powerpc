#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=urbackup-server
PKG_VERSION:=2.5.27
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://hndl.urbackup.org/Server/$(PKG_VERSION)/
PKG_HASH:=f44a3b53d9444c55dce304cae4724a66d43fe112784cfc34790568f511f7dab5

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/urbackup-server
  SECTION:=base
  CATEGORY:=Network
  TITLE:=UrBackup server backup system
  URL:=https://www.urbackup.org
  DEPENDS:=+zlib +libzstd +libcurl +libstdcpp
endef

define Package/urbackup-server/description
  UrBackup is an easy to setup Open Source client/server backup system,
  that through a combination of image and file backups accomplishes both
  data safety and a fast restoration time.
endef

TARGET_CFLAGS += -DCRYPTOPP_DISABLE_ASM

CONFIGURE_ARGS+= \
	--with-zlib="$(STAGING_DIR)/usr" \
	--with-libcurl="$(STAGING_DIR)/usr" \
	--enable-embedded-cryptopp \
	--enable-packaging

define Package/urbackup-server/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/urbackup_mount_helper $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/urbackup_snapshot_helper $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/urbackupsrv $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/var/urbackup
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/var/urbackup/* 	$(1)/var/urbackup/
	$(INSTALL_DIR) $(1)/usr/share/urbackup
	$(CP) $(PKG_INSTALL_DIR)/usr/share/urbackup/* $(1)/usr/share/urbackup/
	$(INSTALL_DIR) $(1)/etc/default
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/defaults_server $(1)/etc/default/urbackupsrv
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/init.d_server $(1)/etc/init.d/urbackupsrv
	$(INSTALL_DIR) $(1)/etc/logrotate.d
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/logrotate_urbackupsrv $(1)/etc/logrotate.d/urbackupsrv
endef

$(eval $(call BuildPackage,urbackup-server))


#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/css
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/fonts
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/images
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/js
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/js/vs
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/js/vs/base
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/js/vs/base/worker
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/js/vs/basic-languages
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/js/vs/basic-languages/src
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/js/vs/editor
#	$(INSTALL_DIR) $(1)/usr/share/urbackup/www/swf

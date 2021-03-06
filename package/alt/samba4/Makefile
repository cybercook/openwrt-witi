#
# Copyright (C) 2007-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=samba4
PKG_VERSION:=4.0.26
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://ftp.samba.org/pub/samba \
	http://ftp.samba.org/pub/samba/stable
PKG_SOURCE:=samba-$(PKG_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/samba-$(PKG_VERSION)
PKG_MD5SUM:=

PKG_LICENSE:=GPLv3
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_DEPENDS:=luci
#PO2LMO:=$(BUILD_DIR)/luci-addons/build/po2lmo
PO2LMO:=$(STAGING_DIR_HOST)/bin/po2lmo

PKG_BUILD_PARALLEL:=1
PKG_BUILD_DEPENDS:=util-linux luci

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

MAKE_PATH:=source3
CONFIGURE_PATH:=source3

PKG_BUILD_BIN:=$(PKG_BUILD_DIR)/$(MAKE_PATH)/bin
define Build/Prepare
	$(Build/Prepare/Default)
	cd $(PKG_BUILD_DIR)/$(CONFIGURE_PATH) && ./autogen.sh
endef

define Package/samba4-server
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Samba $(PKG_VERSION) SMB/CIFS server
  URL:=http://www.samba.org/
  VARIANT:=samba4-server
  DEPENDS:=+USE_EGLIBC:librt +libpthread $(ICONV_DEPENDS) +wsdd +libintl-full
#+libacl +libattr
endef

define Package/samba4-client
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Samba $(PKG_VERSION) SMB/CIFS client
  URL:=http://www.samba.org/
  DEPENDS:=+libphtread +libreadline +libncurses $(ICONV_DEPENDS) +libintl-full
#+libacl +libattr
endef

define Package/samba4-server/config
config PACKAGE_SAMBA4_MAX_DEBUG_LEVEL
	int "Maximum level of compiled-in debug messages"
	depends on PACKAGE_samba4-server || PACKAGE_samba4-client
	default -1
endef

define Package/samba4-server/description
 The Samba software suite is a collection of programs that implements the
 SMB protocol for UNIX systems, allowing you to serve files and printers to
 Windows, NT, OS/2 and DOS clients. This protocol is sometimes also referred
 to as the LanManager or Netbios protocol.
endef

define Package/luci-app-$(PKG_NAME)
  SECTION:=luci
  CATEGORY:=LuCI
  SUBMENU:=3. Applications
  DEPENDS:=+samba4-server
  TITLE:=Samba 4
endef

TARGET_CFLAGS += -DMAX_DEBUG_LEVEL=$(CONFIG_PACKAGE_SAMBA4_MAX_DEBUG_LEVEL) -D_GNU_SOURCE -D__location__=\\\"\\\" -ffunction-sections -fdata-sections -lintl
TARGET_LDFLAGS += -Wl,--gc-sections -lintl

CONFIGURE_VARS += \
	ac_cv_lib_attr_getxattr=no \
	ac_cv_search_getxattr=no \
	ac_cv_file__proc_sys_kernel_core_pattern=yes \
	libreplace_cv_HAVE_C99_VSNPRINTF=yes \
	libreplace_cv_HAVE_GETADDRINFO=yes \
	libreplace_cv_HAVE_IFACE_IFCONF=yes \
	libreplace_cv_HAVE_IFACE_GETIFADDRS=yes \
	libreplace_cv_HAVE_SECURE_MKSTEMP=yes \
	libreplace_cv_REPLACE_INET_NTOA=no \
	libreplace_cv_HAVE_MREMAP=yes \
	libreplace_cv_HAVE_MMAP=yes \
	libreplace_cv_HAVE_OPEN_O_DIRECT=yes \
	LINUX_LFS_SUPPORT=yes \
	samba_cv_big_endian=no \
	samba_cv_little_endian=yes \
	samba_cv_SIZEOF_BLKCNT_T_4=no \
	samba_cv_SIZEOF_BLKCNT_T_8=yes \
	samba_cv_SIZEOF_DEV_T=8 \
	samba_cv_SIZEOF_INO_T=8 \
	samba_cv_SIZEOF_OFF_T=8 \
	samba_cv_SIZEOF_TIME_T=4 \
	samba_cv_TIME_T_MAX=no \
	samba_cv_have_longlong=yes \
	samba_cv_CC_NEGATIVE_ENUM_VALUES=yes \
	samba_cv_HAVE_BROKEN_GETGROUPS=no \
	samba_cv_HAVE_BROKEN_READDIR_NAME=no \
	samba_cv_HAVE_BROKEN_READLINK=no \
	samba_cv_HAVE_KERNEL_CHANGE_NOTIFY=yes \
	samba_cv_HAVE_KERNEL_SHARE_MODES=yes \
	samba_cv_HAVE_MAKEDEV=yes \
	samba_cv_HAVE_GETTIMEOFDAY_TZ=yes \
	samba_cv_HAVE_IFACE_IFCONF=yes \
	samba_cv_HAVE_KERNEL_OPLOCKS_LINUX=yes \
	samba_cv_HAVE_SECURE_MKSTEMP=yes \
	samba_cv_HAVE_WRFILE_KEYTAB=no \
	samba_cv_USE_SETREUID=yes \
	samba_cv_USE_SETRESUID=yes \
	samba_cv_USE_SETRESGID=yes \
	samba_cv_USE_SETREUID=yes \
	samba_cv_have_setresuid=yes \
	samba_cv_have_setreuid=yes \
	samba_cv_have_setresgid=yes \
	samba_cv_linux_getgrouplist_ok=yes \
	ac_cv_header_zlib_h=no \
	samba_cv_zlib_1_2_3=no \
	ac_cv_path_PYTHON="" \
	ac_cv_path_PYTHON_CONFIG=""
CONFIGURE_ARGS += \
	--exec-prefix=/usr \
	--prefix=/ \
	--disable-avahi \
	--disable-cups \
	--disable-pie \
	--disable-relro \
	--disable-static \
	--disable-swat \
	--disable-shared-libs \
	--with-libiconv="$(ICONV_PREFIX)" \
	--with-codepagedir=/etc/samba4 \
	--with-configdir=/etc/samba4 \
	--with-included-iniparser \
	--with-included-popt \
	--with-included-gettext \
	--with-lockdir=/var/lock \
	--with-logfilebase=/var/log \
	--with-nmbdsocketdir=/var/nmbd \
	--with-piddir=/var/run \
	--with-privatedir=/etc/samba4 \
	--with-sendfile-support \
	--without-acl-support \
	--without-cluster-support \
	--without-ads \
	--without-krb5 \
	--without-ldap \
	--without-pam \
	--without-winbind \
	--without-libtdb \
	--without-libtalloc \
	--without-libnetapi \
	--without-libsmbclient \
	--without-libsmbsharemodes \
	--without-libtevent \
	--without-libaddns \
	--with-shared-modules=pdb_tdbsam,pdb_wbc_sam,idmap_nss,nss_info_template,auth_winbind,auth_wbc,auth_domain, vfs \
	--with-static-modules=vfs_default,vfs_dirsort


MAKE_FLAGS += DYNEXP= PICFLAG= MODULES=

define Package/samba4-server/install
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/$(PKG_NAME).config $(1)/etc/config/$(PKG_NAME)
	$(INSTALL_DIR) $(1)/etc/samba4
	$(INSTALL_DATA) ./files/smb.conf.template $(1)/etc/samba4
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/codepages/lowcase.dat $(1)/etc/samba4
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/codepages/upcase.dat $(1)/etc/samba4
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/codepages/valid.dat $(1)/etc/samba4
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/$(PKG_NAME).init $(1)/etc/init.d/$(PKG_NAME)
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_BIN)/$(PKG_NAME)_multicall $(1)/usr/sbin
	ln -sf $(PKG_NAME)_multicall $(1)/usr/sbin/smbd
	ln -sf $(PKG_NAME)_multicall $(1)/usr/sbin/nmbd
	ln -sf $(PKG_NAME)_multicall $(1)/usr/sbin/smbpasswd
	$(INSTALL_DIR) $(1)/usr/bin
	ln -sf ../sbin/$(PKG_NAME)_multicall $(1)/usr/bin/smbcontrol
	ln -sf ../sbin/$(PKG_NAME)_multicall $(1)/usr/bin/smbtree
	ln -sf ../sbin/$(PKG_NAME)_multicall $(1)/usr/bin/smbstatus
	ln -sf ../sbin/$(PKG_NAME)_multicall $(1)/usr/bin/smbcquotas
	ln -sf ../sbin/$(PKG_NAME)_multicall $(1)/usr/bin/smbcacls
	ln -sf ../sbin/$(PKG_NAME)_multicall $(1)/usr/bin/nmblookup
	ln -sf ../sbin/$(PKG_NAME)_multicall $(1)/usr/bin/testparm
	$(INSTALL_DIR) $(1)/etc/samba4
	ln -sf /tmp/etc/secrets.tdb $(1)/etc/samba4/secrets.tdb
	ln -sf /tmp/etc/schannel_store.tdb $(1)/etc/samba4/schannel_store.tdb
endef

define Package/samba4-server/conffiles
/etc/config/samba4
/etc/samba4/smb.conf.template
endef

define Package/samba4-client/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_BIN)/smbclient $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_BIN)/nmblookup $(1)/usr/sbin
endef

define Package/luci-app-$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller
	$(INSTALL_DATA) files/$(PKG_NAME).controller.lua $(1)/usr/lib/lua/luci/controller/$(PKG_NAME).lua
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/model/cbi
	$(INSTALL_DATA) files/$(PKG_NAME).cbi.lua $(1)/usr/lib/lua/luci/model/cbi/$(PKG_NAME).lua
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
	for transl in files/*.po; do \
		transl_dest=`basename $$$$transl .po`.lmo; \
		$(PO2LMO) $$$$transl $(1)/usr/lib/lua/luci/i18n/$$$$transl_dest; \
	done
endef

$(eval $(call BuildPackage,$(PKG_NAME)-client))
$(eval $(call BuildPackage,$(PKG_NAME)-server))
$(eval $(call BuildPackage,luci-app-$(PKG_NAME)))

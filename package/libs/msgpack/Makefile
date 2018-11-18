include $(TOPDIR)/rules.mk

PKG_NAME:=libmsgpack
PKG_VERSION:=2.1.5
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
#PKG_SOURCE_PROTO:=git
#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=https://github.com/msgpack/msgpack-c/releases/download/cpp-2.1.5/
# PKG_MD5SUM:=705106a9378c792fe22d285dba5c142c
#PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libmsgpack
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Message Pack library
  URL:=http://msgpack.org
endef

define Package/libmsgpack/description
  MessagePack is an efficient binary serialization format.
  It lets you exchange data among multiple languages like JSON but it's faster and smaller.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	tar -zxf src.tar.gz -C $(PKG_BUILD_DIR)/ --strip 1
	#$(CP) ./src/* $(PKG_BUILD_DIR)/	
	rm -f $(PKG_BUILD_DIR)/CMakeCache.txt
	rm -fR $(PKG_BUILD_DIR)/CMakeFiles
	rm -f $(PKG_BUILD_DIR)/Makefile
	rm -f $(PKG_BUILD_DIR)/cmake_install.cmake
	rm -f $(PKG_BUILD_DIR)/progress.make
endef

define Build/Configure
	IN_OPENWRT=1 \
	AR="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)ar" \
	AS="$(TOOLCHAIN_DIR)/bin/$(TARGET_CC) -c $(TARGET_CFLAGS)" \
	LD="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)ld" \
	NM="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)nm" \
	CC="$(TOOLCHAIN_DIR)/bin/$(TARGET_CC)" \
	GCC="$(TOOLCHAIN_DIR)/bin/$(TARGET_CC)" \
	CXX="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)g++" \
	RANLIB="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)ranlib" \
	STRIP="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)strip" \
	OBJCOPY="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)objcopy" \
	OBJDUMP="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)objdump" \
	TARGET_CPPFLAGS="$(TARGET_CPPFLAGS)" \
	TARGET_CFLAGS="$(TARGET_CFLAGS)" \
	TARGET_LDFLAGS="$(TARGET_LDFLAGS)" \
	cmake $(PKG_BUILD_DIR)/CMakeLists.txt
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR)/usr/include/libmsgpack
	$(CP) $(PKG_BUILD_DIR)/include/* $(STAGING_DIR)/usr/include/
	$(INSTALL_DIR) $(STAGING_DIR)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libmsgpackc.{a,so*} $(STAGING_DIR)/usr/lib/
endef

define Build/UninstallDev
	rm -rf \
	$(STAGING_DIR)/usr/include/libmsgpack \
	$(STAGING_DIR)/usr/lib/libmsgpackc.{a,so*}
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)
endef

#define Build/Configure
#     $(call Build/Configure/Default, )
#endef
define Package/libmsgpack/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libmsgpackc.so* $(1)/usr/lib/
endef
$(eval $(call BuildPackage,libmsgpack))

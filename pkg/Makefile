include $(TOPDIR)/rules.mk

PKG_NAME:=apfree-wifidog
PKG_VERSION:=9.11.2969
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/liudf0716/apfree-wifidog.git
PKG_SOURCE_VERSION:=f012073104f2a1aa75954b5bc0e013b14bf663d4
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=COPYING
PKG_MAINTAINER:=ThalynnX

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/apfree-wifidog
  SECTION:=net
  CATEGORY:=Network
  TITLE:=ApFree WiFiDog captive portal
  URL:=https://github.com/liudf0716/apfree-wifidog
  DEPENDS:=+libuci +libevent2 +libevent2-core +libevent2-openssl +libevent2-extra +libopenssl +libjson-c +libmosquitto-ssl +libnetfilter-queue +libnftnl +libmnl +zlib
endef

define Package/apfree-wifidog/description
 High-performance captive portal (HTTP/HTTPS) for OpenWrt.
endef

CMAKE_OPTIONS += \
	-DAW_AUTO_INSTALL_DEPS=OFF \
	-DAW_FW3=OFF \
	-DENABLE_XDPI_FEATURE=ON \
	-DENABLE_REMOTE_SHELL=OFF


# 源码补丁：api_handlers_bpf.c 里的 BPF_ANY/BPF_NOEXIST/BPF_EXIST 与本机 linux/bpf.h
# 中的同名 enum 冲突，去掉自定义宏并直接引入内核头文件。
define Build/Prepare
	$(call Build/Prepare/Default)
	$(SED) '/^#define BPF_ANY /d' $(PKG_BUILD_DIR)/src/api_handlers_bpf.c
	$(SED) '/^#define BPF_NOEXIST /d' $(PKG_BUILD_DIR)/src/api_handlers_bpf.c
	$(SED) '/^#define BPF_EXIST /d' $(PKG_BUILD_DIR)/src/api_handlers_bpf.c
	$(SED) '1i #include <linux/bpf.h>' $(PKG_BUILD_DIR)/src/api_handlers_bpf.c
	grep -n "linux/bpf.h" $(PKG_BUILD_DIR)/src/api_handlers_bpf.c
endef

define Package/apfree-wifidog/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/wifidogx $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/wdctlx $(1)/usr/bin/
endef

$(eval $(call BuildPackage,apfree-wifidog))

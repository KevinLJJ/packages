#
# Copyright (C) 2021 ImmortalWrt
# <https://immortalwrt.org>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=webdav-go
PKG_VERSION:=4.1.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/hacdias/webdav/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=de968329b0429b7f2c312d69706762b210240a813e0b939009e9480c7c09d86d

PKG_LICENSE:=MIT
PKG_LICENSE_FILE:=LICENSE
PKG_MAINTAINER:=ElonH <elonhhuang@gmail.com>

PKG_CONFIG_DEPENDS:= \
	CONFIG_WEBDAV_GO_COMPRESS_GOPROXY \
	CONFIG_WEBDAV_GO_COMPRESS_UPX

PKG_BUILD_DIR:=$(BUILD_DIR)/$(firstword $(subst -, ,$(PKG_NAME)))-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=github.com/hacdias/webdav
GO_PKG_LDFLAGS:=-s -w

include $(INCLUDE_DIR)/package.mk
include ../../lang/golang/golang-package.mk

define Package/webdav-go
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=File Transfer
  TITLE:=Simple Go WebDAV server
  URL:=https://github.com/hacdias/webdav
  DEPENDS:=$(GO_ARCH_DEPENDS)
endef

define Package/webdav-go/description
  WebDAV command line interface is really easy to use so you can easily
  create a WebDAV server for your own user. By default, it runs on a
  random free port and supports JSON, YAML and TOML configuration.
endef

define Package/webdav-go/config
config WEBDAV_GO_COMPRESS_GOPROXY
	bool "Compiling with GOPROXY proxy"
	default n

config WEBDAV_GO_COMPRESS_UPX
	bool "Compress executable files with UPX"
	depends on !mips64
	default y if mips||mipsel
endef

ifeq ($(CONFIG_WEBDAV_GO_COMPRESS_GOPROXY),y)
	export GO111MODULE=on
	export GOPROXY=https://goproxy.baidu.com
endif

define Build/Compile
	$(call GoPackage/Build/Compile)
ifeq ($(CONFIG_WEBDAV_GO_COMPRESS_UPX),y)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/webdav
endif
endef

define Package/webdav-go/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/webdav $(1)/usr/bin/webdav-go
endef

$(eval $(call GoBinPackage,webdav-go))
$(eval $(call BuildPackage,webdav-go))

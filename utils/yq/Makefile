include $(TOPDIR)/rules.mk

PKG_NAME:=yq
PKG_VERSION:=4.14.2
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/mikefarah/yq/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=2917a72bc0cb0fbd132b3257ff9162db83d129adc5670f7661c29a873684e04a

PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

PKG_CONFIG_DEPENDS:= \
	CONFIG_YQ_COMPRESS_GOPROXY \
	CONFIG_YQ_COMPRESS_UPX

GO_PKG:=github.com/mikefarah/yq
GO_PKG_LDFLAGS:=-s -w

include $(INCLUDE_DIR)/package.mk
include ../../lang/golang/golang-package.mk

define Package/yq
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Portable command-line YAML processor
  URL:=https://mikefarah.gitbook.io/yq/
  DEPENDS:=$(GO_ARCH_DEPENDS)
endef

define Package/yq/description
  The aim of the project is to be the jq or sed of yaml files.
endef

define Package/yq/config
config YQ_COMPRESS_GOPROXY
	bool "Compiling with GOPROXY proxy"
	default n

config YQ_COMPRESS_UPX
	bool "Compress executable files with UPX"
	depends on !mips64
	default y if mips||mipsel
endef

ifeq ($(CONFIG_YQ_COMPRESS_GOPROXY),y)
export GO111MODULE=on
export GOPROXY=https://goproxy.baidu.com
endif

define Build/Compile
	$(call GoPackage/Build/Compile)
ifeq ($(CONFIG_YQ_COMPRESS_UPX),y)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/yq
endif
endef

$(eval $(call GoBinPackage,yq))
$(eval $(call BuildPackage,yq))

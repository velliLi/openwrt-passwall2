# Copyright (C) 2022-2025 xiaorouji
# Copyright (C) 2026 Openwrt-Passwall Organization
#
# This is free software, licensed under the GNU General Public License v3.

include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-passwall2
PKG_VERSION:=26.4.20
PKG_RELEASE:=1
PKG_PO_VERSION:=$(PKG_VERSION)

#去除不需要的协议
PKG_CONFIG_DEPENDS:= \
	CONFIG_PACKAGE_$(PKG_NAME)_Nftables_Transparent_Proxy \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_Haproxy \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_IPv6_Nat \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_SingBox

LUCI_TITLE:=LuCI support for PassWall 2
LUCI_PKGARCH:=all
#去除XrayCore
LUCI_DEPENDS:=+curl \
	+ip-full +libuci-lua +lua +luci-compat +luci-lib-jsonc +resolveip +tcping \
	+geoview +v2ray-geoip +v2ray-geosite\
	+unzip 

define Package/$(PKG_NAME)/config
menu "Configuration"

config PACKAGE_$(PKG_NAME)_INCLUDE_IPv6_Nat
	depends on PACKAGE_ip6tables
	bool "Include IPv6 Nat"
	default n

if PACKAGE_$(PKG_NAME)

config PACKAGE_$(PKG_NAME)_Nftables_Transparent_Proxy
	bool "Nftables Transparent Proxy"
	select PACKAGE_chinadns-ng
	select PACKAGE_dnsmasq-full
	select PACKAGE_dnsmasq_full_nftset
	select PACKAGE_nftables
	select PACKAGE_kmod-nft-socket
	select PACKAGE_kmod-nft-tproxy
	select PACKAGE_kmod-nft-nat
	default y if PACKAGE_firewall4

config PACKAGE_$(PKG_NAME)_INCLUDE_Haproxy
	bool "Include Haproxy"
	select PACKAGE_haproxy
	default y if aarch64||arm||i386||x86_64

config PACKAGE_$(PKG_NAME)_INCLUDE_SingBox
	bool "Include Sing-Box"
	select PACKAGE_sing-box
	default y if aarch64||arm||i386||x86_64

#删除SS、Simple_Obfs、tuic_client、V2ray_Plugin、Hysteria、NaiveProxy

endif
endmenu
endef

define Package/$(PKG_NAME)/conffiles
/etc/config/passwall2
/etc/config/passwall2_server
/usr/share/passwall2/domains_excluded
endef

include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature

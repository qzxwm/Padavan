SRC_NAME = ZeroTierOne-1.10.2
THISDIR = $(shell pwd)
FLAGS := DEFS=""
CFLAGS := -Os -ffunction-sections -fdata-sections
LDFLAGS := -Wl,--gc-sections,--as-needed

all: extract_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

extract_test:
	 如果 [ ！-d $(SRC_NAME) ] ; 然后\
		tar xf $(SRC_NAME).tar.gz ; \
		补丁 -d $(SRC_NAME) -p1 -i ../0001-fix-makefile.patch ; \
		补丁 -d $(SRC_NAME) -p1 -i ../0002-fix-compilation-for-arm_cortex-a7-neon.patch ; \
		补丁 -d $(SRC_NAME) -p1 -i ../0006-gcc10.patch ; \
		补丁 -d $(SRC_NAME) -p1 -i ../0007-add-cerrno-header-for-str-errno.patch ; \
		补丁 -d $(SRC_NAME) -p1 -i ../0009-fix-set-mac-and-change-confpath.patch ; \
	 菲 ;

配置测试：
干净的：
	如果 [ -f $(SRC_NAME)/Makefile ] ; 然后\
		$(MAKE) -C $(SRC_NAME) distclean ; \
	菲 ; \

ROMFS：
	$(ROMFSINST) -p +x $(THISDIR)/$(SRC_NAME)/zerotier-one /usr/bin/zerotier-one
	$(ROMFSINST) -p +x $(THISDIR)/zerotier.sh /usr/bin/zerotier.sh
	ln -sf zerotier-one $(ROMFSDIR)/usr/bin/zerotier-cli
	ln -sf zerotier-one $(ROMFSDIR)/usr/bin/zerotier-idtool
	cp -fP $(CONFIG_CROSS_COMPILER_ROOT)/mipsel-linux-uclibc/sysroot/lib/libstdc++.so* $(ROMFSDIR)/lib


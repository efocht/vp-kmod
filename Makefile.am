#
# VP Kernel Module
#
# Copyright (C) 2017-2018 NEC Corporation
# This file is part of VP Kernel Module.
#
# VP Kernel Module is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# VP Kernel Module is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public
# License along with the VP Kernel Module; if not, see
# <http://www.gnu.org/licenses/>.
#
ACLOCAL_AMFLAGS = -I m4
include_HEADERS = src/vp.h
EXTRA_DIST = src/Makefile src/main.c src/compat.c src/compat.h src/vp.h \
	     src/vp.conf src/commitid.h dracut/vp.conf \
	     vp-kmod.spec \
	     debian
bits=16

src/commitid.h:
	echo "#define COMMITID \"`git log --pretty=format:"%H" -n 1`\"" >> $@

src/vp.h:
	sed -e "s/#define HASH_LIST_BITS 11/#define HASH_LIST_BITS $(bits)/" src/vp.h.in > src/vp.h

src/vp.ko: src/commitid.h src/vp.h
	$(MAKE) -C $(KERNEL_SRC) M=$(abs_top_builddir)/src

src/Module.symvers: src/vp.ko

all-local: src/vp.ko

install-exec-hook: src/vp.ko
	$(MKDIR_P) $(DESTDIR)$(KERNEL_MOD)
	$(INSTALL_DATA) $(builddir)/src/vp.ko $(DESTDIR)$(KERNEL_MOD)/
	$(MKDIR_P) $(DESTDIR)/etc/modprobe.d/
	$(INSTALL_DATA) $(builddir)/src/vp.conf $(DESTDIR)/etc/modprobe.d/
	$(MKDIR_P) $(DESTDIR)@prefix@/src/ve_kernel/
	$(INSTALL_DATA) $(builddir)/src/Module.symvers $(DESTDIR)@prefix@/src/ve_kernel/
	$(MKDIR_P) $(DESTDIR)/etc/dracut.conf.d/
	$(INSTALL_DATA) $(builddir)/dracut/vp.conf $(DESTDIR)/etc/dracut.conf.d/

clean-local:
	rm -rf src/vp.h
	$(MAKE) -C $(KERNEL_SRC) M=$(abs_top_builddir)/src clean

rpm: dist
	mkdir -p $(distdir)/rpmbuild/{BUILD,RPMS,SOURCES,SRPMS,SPECS}
	rpmbuild -ta --clean --define='_topdir $(CURDIR)/$(distdir)/rpmbuild' ${distdir}.tar.gz

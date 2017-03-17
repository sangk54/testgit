#$L$
# Copyright (C) 2012-2013 Ridgerun (http://www.ridgerun.com). 
##$L$

PKG_URL=http://www.python.org/ftp/python/2.7.2/
PKG_TARBALL=Python-2.7.2.tar.bz2
PKG_SHA1SUM=417bdeea77abfaf1b9257fc6b4a04aaa209f4547
FETCHER_EXTRACT_DIRECTORY_NAME=src

include ../../../bsp/classes/rrsdk.class
include $(CLASSES)/flags.defs
include $(CLASSES)/fetcher.defs

HEADERS=$(shell find $(FSDEVROOT)/usr/include/python${PYTHON_VERSION}/* -printf "%f,"  | sed 's/.\{1\}$$//')

BINARIES=/usr/bin/python{,${PYTHON_VERSION}{,-config},-config}
LIBRARIES=/usr/lib/libpython${PYTHON_VERSION}.so{,.1.0}
OTHERS=/usr/include/python${PYTHON_VERSION}/{${HEADERS}}

LIBRARIES_OPTIONAL=$(shell find $(FSDEVROOT)/usr/lib/python${PYTHON_VERSION}/* -name '*.pyo' -or -name '*.so' -or \
 ! -name '*.*' -type f -not -path '*/idlelib/*' -not -path '*/lib2to3/*' -not -path '*/lib-tk/*' | \
 grep -v "testcode" | grep -v "/tests/" | grep -v "/test/" | grep -v "unittest" | grep -v "doctest" \
 | grep -v "_testcapi.so" | grep -v "_ctypes_test.so" | sed 's!.*fsdev!!' )

PYTHON_HOST_BIN=$(HOSTAPPSDIR)/python*/$(HOST_ARCH)/python
PYTHON_HOST_PGEN=$(HOSTAPPSDIR)/python*/$(HOST_ARCH)/Parser/pgen

build: rrfetched rrpatched rrbuilt 

rrbuilt: configured py_build
	touch $@

configured:
	$(V) cp -r $(FETCHER_EXTRACT_DIRECTORY_NAME) $(ARCH) $(QOUT)
ifeq "$(VERBOSE)" "0" 
	$(V) cd $(ARCH) && \
	./configure \
	--build=$(HOST_ARCH) --host=$(TOOLCHAIN_PREFIX) --prefix=/usr \
	--with-threads \
	--with-pymalloc \
	--with-signal-module \
	--with-wctype-functions \
	--enable-unicode=ucs4 \
	--enable-shared 2>&1 > /dev/null
else
	$(V) cd $(ARCH) && \
	./configure \
	--build=$(HOST_ARCH) --host=$(TOOLCHAIN_PREFIX) --prefix=/usr \
	--with-threads \
	--with-pymalloc \
	--with-signal-module \
	--with-wctype-functions \
	--enable-unicode=ucs4 \
	--enable-shared
	$(V) touch $@
endif

host-built:
	$(V) \
	if [ ! -e $(PYTHON_HOST_BIN) ] ; then \
	  $(ECHO) "$(ERROR_COLOR)Error:$(NORMAL_COLOR) Can't find $(PYTHON_HOST_BIN)" ; \
	  $(ECHO) "Please make sure that python for the host is built first.\n" ; \
	  exit -1 ; \
	fi ; \
	if [ ! -e $(PYTHON_HOST_PGEN) ] ; then \
	  $(ECHO) "$(ERROR_COLOR)Error:$(NORMAL_COLOR) Can't find $(PYTHON_HOST_PGEN)" ; \
	  $(ECHO) "Please make sure that python for the host is built first.\n" ; \
	  exit -1 ; \
	fi 
	$(V) touch $@

clean:
	$(V) rm -rf rrbuilt py_build

distclean: unpatch
	$(V) rm -rf host-built rrfetched rrpatched series src/ py_build arm/ configured

PY_BUILD_CMD=HOSTPYTHON=$(PYTHON_HOST_BIN) HOSTPGEN=$(PYTHON_HOST_PGEN) \
BLDSHARED="$(TOOLCHAIN_PREFIX)-gcc -shared" CROSS_COMPILE=$(TOOLCHAIN_PREFIX)- \
CROSS_COMPILE_TARGET=yes HOSTARCH=$(TOOLCHAIN_PREFIX) BUILDARCH=$(HOST_ARCH)

PY_INST_CMD=HOSTPYTHON=$(PYTHON_HOST_BIN) BLDSHARED="$(TOOLCHAIN_PREFIX)-gcc -shared" \
CROSS_COMPILE=$(TOOLCHAIN_PREFIX)- CROSS_COMPILE_TARGET=yes prefix=$(FSDEVROOT)/usr


py_build:
ifeq "$(VERBOSE)" "0" 
	$(V) $(MAKE) -j $(BSP_NCPU) -C $(ARCH) $(PY_BUILD_CMD) 2>&1 > /dev/null
	$(V) $(MAKE) -C $(ARCH) install $(PY_INST_CMD) 2>&1 > /dev/null
else
	$(V) $(MAKE) -j $(BSP_NCPU) -C $(ARCH) $(PY_BUILD_CMD)
	$(V) $(MAKE) -C $(ARCH) install $(PY_INST_CMD)
endif
	touch $@

install: rrsdk_install

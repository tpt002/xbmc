#  A quick and dirty Makefile to download/build and install
#
#  Usage:
#    make
#    sudo make install
#

include ../config.mk

# lib name, version
LIBNAME=Python
VERSION=2.6.5
SOURCE=$(LIBNAME)-$(VERSION)
# download location and format
BASE_URL=http://mirrors.xbmc.org/build-deps/darwin-libs
ARCHIVE=$(SOURCE).tar.bz2

RETRIEVE_TOOL=/usr/bin/curl
RETRIEVE_TOOL_FLAGS=-Ls --output $(ARCHIVE)
ARCHIVE_TOOL=tar
ARCHIVE_TOOL_FLAGS=xf

# configuration settings
export EXTRA_CFLAGS=$(CFLAGS)
CONFIGURE=./configure --prefix=$(PREFIX) --host=$(HOST) --enable-shared \
  --disable-toolbox-glue

LIBDYLIB=$(SOURCE)/libpython2.6.dylib

CLEAN_FILES=$(ARCHIVE) $(SOURCE)

all: $(LIBDYLIB) .installed

$(ARCHIVE):
	$(RETRIEVE_TOOL) $(RETRIEVE_TOOL_FLAGS) $(BASE_URL)/$(ARCHIVE)
	echo $(ARCHIVE) > .gitignore

$(LIBDYLIB): $(ARCHIVE)
	rm -rf $(SOURCE)
	$(ARCHIVE_TOOL) $(ARCHIVE_TOOL_FLAGS) $(ARCHIVE)
	echo $(SOURCE) >> .gitignore
	cd $(SOURCE); $(CONFIGURE)
	cd $(SOURCE); make

.installed:
	cd $(SOURCE); make install
	find $(PREFIX)/lib/python2.6 -type f -name *.pyc -exec rm -f {} \;	
	find $(PREFIX)/lib/python2.6 -type f -name *.pyo -exec rm -f {} \;	
	touch $@

clean:
	rm -rf $(SOURCE) .installed

distclean::
	rm -rf $(SOURCE) .installed


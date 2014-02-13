#!/bin/sh

# (C) 2012-2013 Fathi Boudra <fathi.boudra@linaro.org>
# (C) 2014 Ryoichi KATO <ryo1kato@gmail.com>

# Calls all necessary live-build programs in the correct order to complete
# the bootstrap, chroot, binary, and source stage.

# You need live-build package installed and superuser privileges.

BUILD_NUMBER ?= 1
BUILD_DATE = $(shell [ -e .builddate ] && cat .builddate || date +%Y%m%d | tee .builddate )

IMAGEPREFIX=linaro-saucy-nano-$(BUILD_DATE)-$(BUILD_NUMBER)
LOGFILE=$(IMAGEPREFIX).build-log.txt
CONFIGFILE=$(IMAGEPREFIX).config.tar.bz2
LISTFILE=$(IMAGEPREFIX).contents
PKGSFILE=$(IMAGEPREFIX).packages
TARGZFILE=$(IMAGEPREFIX).tar.gz
MD5SUMSFILE=$(IMAGEPREFIX).md5sums.txt
SHA1SUMSFILE=$(IMAGEPREFIX).sha1sums.txt

FILES_TO_HASH=$(LOGFILE) $(CONFIGFILE) $(LISTFILE) $(PKGSFILE) $(TARGZFILE)

all:
	script -e -c 'sudo lb build' $(LOGFILE)
	if [ -f binary-tar.tar.gz ]; then \
		tar -jcf $(CONFIGFILE) auto/ config/ configure; \
		sudo mv binary.contents $(LISTFILE); \
		sudo mv chroot.packages.live $(PKGSFILE); \
		sudo mv binary-tar.tar.gz $(TARGZFILE); \
		md5sum $(FILES_TO_HASH) > $(MD5SUMSFILE); \
		sha1sum $(FILES_TO_HASH) > $(SHA1SUMSFILE); \
	fi

clean:
	sudo lb clean --purge
	rm -f $(IMAGEPREFIX).*

cleanall: clean
	rm -f .builddate
	rm -rf config local
	sudo rm -rf chroot

distclean: cleanall

.PHONY: all clean cleanall distclean

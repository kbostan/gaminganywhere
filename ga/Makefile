
# Makefile for POSIX-based OSes
GADEPS	?= $(HOME)/gaminganywhere/deps.posix
PKG_CONFIG_PATH	=  $(GADEPS)/lib/pkgconfig

TARGET	= core module server client

.PHONY: $(TARGET)

all: $(TARGET)

core:
	PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)" make -C core

module: core
	PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)" make -C module

server: core
	PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)" make -C server

client: core
	PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)" make -C client #fix

install:
	mkdir -p ../bin
	make -C core install
	make -C module install
	make -C server install
	make -C client install
	cp -fr config ../bin/

msys2-dist:
	mkdir -p ../bin-dist
	cp -fr ../bin/* ../bin-dist/
	cp `ldd ../bin/*.exe ../bin/*.dll | grep -Ei '/mingw.*/bin' | awk '{print $$3}' | sort | uniq` ../bin-dist/

clean:
	make -C core clean
	make -C module clean
	make -C server clean
	make -C client clean


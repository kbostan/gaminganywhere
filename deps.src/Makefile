
GADEPS		?= $(HOME)/trial/gaminganywhere/deps.posix
MACHINE		?= $(shell uname -m)
PKG_CONFIG_PATH	=  $(GADEPS)/lib/pkgconfig
USE_SYSTEM_PACKAGES ?= 0
PARALLEL_MAKE	?= -j5
SYSTEM		= $(shell uname -s)
PACKAGES	= freetype cmake sdl2 sdl2_ttf yasm nasm lame opus ogg vorbis theora vpx \
		  x264 x265 xvid ffmpeg live555 openssl
OSX_CLANG_PATCH	= echo '*** Not on OS X: not applied ***'

### package configurations for Mac
ifeq ($(SYSTEM),Darwin)
VPX_EXTRA_CONFIG =
HAS_BREW	?= $(shell if which -s brew; then echo 1; else echo 0; fi)
MACHINE		= $(shell uname -m)-apple-darwin
OSX_CLANG_PATCH = perl -p -i -e "s/-falign-loops=16//g" configure; \
		  perl -p -i -e "s/-fforce-addr//g"     configure
ifeq ($(HAS_BREW),1)
PACKAGES	= osx_brew live555
endif
else
VPX_EXTRA_CONFIG = --enable-shared
endif

### package configurations for Linux
ifeq ($(SYSTEM),Linux)
###### Ubuntu?
LINUXDIST	= $(shell grep '=Ubuntu' /etc/lsb-release | awk -F= '{print $$2}')
ifeq ($(LINUXDIST),Ubuntu)
PACKAGES	= linux_ubuntu x265 ffmpeg live555
endif
endif

TBZUNPACK	= tar xjf
TGZUNPACK	= tar xzf

include Makefile.packages

all: $(PACKAGES)

osx_brew:
	brew install sdl2 sdl2_ttf yasm lame opus libogg libvorbis theora \
		libvpx x264 x265 xvid
	brew install --with-libvorbis --with-libvpx --with-opus \
		--with-theora --with-x265 --with-tools ffmpeg

linux_ubuntu:
	@echo 'You will need root permissions to install packages'
	sudo apt-get install libsdl2-dev libsdl2-ttf-dev yasm \
			libmp3lame-dev libopus-dev libogg-dev \
			libvorbis-dev libtheora-dev \
			libvpx-dev libx264-dev libxvidcore-dev
			
clean:
	rm -rf $(GADEPS)

sdl2:
	$(TGZUNPACK) $(SDL2)
	mkdir -p $(SDL2DIR)/mybuild
	cd $(SDL2DIR)/mybuild; ../configure --prefix=$(GADEPS) --with-pic
	make -C $(SDL2DIR)/mybuild $(PARALLEL_MAKE)
	make -C $(SDL2DIR)/mybuild install
	rm -rf $(SDL2DIR)

sdl2_ttf:
	$(TGZUNPACK) $(SDL2_TTF)
	mkdir -p $(SDL2_TTFDIR)/mybuild
	cd $(SDL2_TTFDIR)/mybuild; PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)" ../configure --prefix=$(GADEPS) --with-pic
	make -C $(SDL2_TTFDIR)/mybuild $(PARALLEL_MAKE)
	make -C $(SDL2_TTFDIR)/mybuild install
	rm -rf $(SDL2_TTFDIR)

yasm:
	$(TGZUNPACK) $(YASM)
	cd $(YASMDIR); ./configure --prefix=$(GADEPS)
	make -C $(YASMDIR) $(PARALLEL_MAKE)
	make -C $(YASMDIR) install
	rm -rf $(YASMDIR)
	
nasm:
	$(TGZUNPACK) $(NASM)
	cd $(NASMDIR); ./configure --prefix=$(GADEPS)
	make -C $(NASMDIR) $(PARALLEL_MAKE)
	make -C $(NASMDIR) install
	rm -rf $(NASMDIR)

lame:
	$(TGZUNPACK) $(LAME)
	cd $(LAMEDIR); ./configure --prefix=$(GADEPS) --with-pic
	make -C $(LAMEDIR) $(PARALLEL_MAKE)
	make -C $(LAMEDIR) install
	rm -rf $(LAMEDIR)

opus:
	$(TGZUNPACK) $(OPUS)
	#
	cd $(OPUSDIR); ./configure --prefix=$(GADEPS) --with-pic
	make -C $(OPUSDIR) $(PARALLEL_MAKE)
	make -C $(OPUSDIR) install
	rm -rf $(OPUSDIR)

ogg:
	$(TGZUNPACK) $(OGG)
	cd $(OGGDIR); ./configure --prefix=$(GADEPS) --with-pic
	make -C $(OGGDIR) $(PARALLEL_MAKE)
	make -C $(OGGDIR) install
	rm -rf $(OGGDIR)

vorbis:
	$(TGZUNPACK) $(VORBIS)
	cd $(VORBISDIR); ./configure --prefix=$(GADEPS) --build=$(MACHINE) --with-pic
	make -C $(VORBISDIR) $(PARALLEL_MAKE)
	make -C $(VORBISDIR) install
	rm -rf $(VORBISDIR)

theora:
	$(TBZUNPACK) $(THEORA)
	cd $(THEORADIR); $(OSX_CLANG_PATCH)
	cd $(THEORADIR); PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)" ./configure --prefix=$(GADEPS) --with-pic --disable-examples
	make -C $(THEORADIR) $(PARALLEL_MAKE)
	make -C $(THEORADIR) install
	rm -rf $(THEORADIR)

vpx:
	$(TGZUNPACK) $(VPX)
	cd $(VPXDIR); PATH="$(GADEPS)/bin:$(PATH)" PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)" ./configure \
	--prefix=$(GADEPS) --libdir=$(GADEPS)/lib $(VPX_EXTRA_CONFIG) --enable-pic
	PATH="$(GADEPS)/bin:$(PATH)" make -C $(VPXDIR) $(PARALLEL_MAKE)
	make -C $(VPXDIR) install
	rm -rf $(VPXDIR)

x264:
	$(TBZUNPACK) $(X264)
	cd $(X264DIR); $(OSX_CLANG_PATCH)
	cd $(X264DIR); PATH="$(GADEPS)/bin:$(PATH)" ./configure --prefix=$(GADEPS) --enable-shared --enable-pic
	PATH="$(GADEPS)/bin:$(PATH)" make -C $(X264DIR) $(PARALLEL_MAKE)
	make -C $(X264DIR) install
	rm -rf $(X264DIR)

x265:
	$(TGZUNPACK) $(X265)
	cd $(X265DIR)/build/linux; PATH="$(GADEPS)/bin:$(PATH)" cmake \
		-D CMAKE_INSTALL_PREFIX:PATH=$(GADEPS) \
		-D CMAKE_CXX_FLAGS="-fPIC" -D CMAKE_C_FLAGS="-fPIC" \
		../../source/
	make -C $(X265DIR)/build/linux $(PARALLEL_MAKE)
	make -C $(X265DIR)/build/linux install
	rm -rf $(X265DIR)

xvid:
	$(TBZUNPACK) $(XVID)
	cd $(XVIDDIR)/build/generic; ./configure --prefix=$(GADEPS)
	make -C $(XVIDDIR)/build/generic $(PARALLEL_MAKE)
	make -C $(XVIDDIR)/build/generic install
	rm -rf $(XVIDDIR)

ffmpeg:### update with nvenc and so on..
	$(TBZUNPACK) $(FFMPEG)
	cd $(FFMPEGDIR); PATH="$(HOME)/bin:$(GADEPS)/bin:$(PATH)" PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)" ./configure \
		--prefix=$(GADEPS) \
		--pkg-config-flags="--static" \
		--extra-cflags="-I$(GADEPS)/include -I$(GADEPS)/include/vorbis" \
		--extra-ldflags="-L$(GADEPS)/lib" \
		--extra-libs="-lpthread -lm" \
		--ld="g++" \
		--enable-shared --enable-pic \
		--enable-libmp3lame \
		--enable-libtheora --enable-libvorbis \
		--enable-libx264 --enable-libvpx \
		--enable-libx265 \
		--enable-libopus \
		--enable-libxvid \
		--enable-gpl --enable-nonfree --disable-outdev=sdl
	PATH="$(GADEPS)/bin:$(PATH)" make -C $(FFMPEGDIR) $(PARALLEL_MAKE)
	make -C $(FFMPEGDIR) install
	rm -rf $(FFMPEGDIR)

live555:
	$(TGZUNPACK) $(LIVE555)
	# enable -fPIC for macosx and linux
	mv -f $(LIVE555DIR)/config.linux $(LIVE555DIR)/config.linux.old
	cat $(LIVE555DIR)/config.linux.old | sed -e 's,^COMPILE_OPTS.*$$,& -fPIC,' > $(LIVE555DIR)/config.linux
	#
	cd $(LIVE555DIR); ./genMakefiles $(shell uname -s | tr A-Z a-z | sed -e 's,darwin,macosx,' -e 's,cygwin.*,cygwin,')
	make -C $(LIVE555DIR) $(PARALLEL_MAKE)
	-mkdir -p $(GADEPS)/lib
	find $(LIVE555DIR) -name '*.a' -exec cp -f {} $(GADEPS)/lib \;
	-mkdir -p $(GADEPS)/include/live555
	find $(LIVE555DIR) -name '*.hh' -exec cp -f {} $(GADEPS)/include/live555 \;
	cp -f $(LIVE555DIR)/groupsock/include/NetCommon.h $(GADEPS)/include/live555
	rm -rf $(LIVE555DIR)
	
freetype:
	$(TGZUNPACK) $(FREETYPE)
	mkdir -p $(FREETYPEDIR)/mybuild
	cd $(FREETYPEDIR)/mybuild; ../configure --prefix=$(GADEPS) --with-pic
	make -C $(FREETYPEDIR)/mybuild $(PARALLEL_MAKE)
	make -C $(FREETYPEDIR)/mybuild install
	rm -rf $(FREETYPEDIR)
	ln -s $(GADEPS)/lib/pkgconfig $(GADEPS)/lib/freetype-config 
	
cmake:
	$(TGZUNPACK) $(CMAKE)
	mkdir -p $(CMAKEDIR)/mybuild
	cd $(CMAKEDIR)/mybuild; ../configure --prefix=$(GADEPS)
	make -C $(CMAKEDIR)/mybuild $(PARALLEL_MAKE)
	make -C $(CMAKEDIR)/mybuild install
	rm -rf $(CMAKEDIR)
	
openssl:
	$(TGZUNPACK) $(OPENSSL)
	mkdir -p $(OPENSSLDIR)/mybuild
	cd $(OPENSSLDIR)/mybuild; ../config --prefix=$(GADEPS)
	make -C $(OPENSSLDIR)/mybuild $(PARALLEL_MAKE)
	make -C $(OPENSSLDIR)/mybuild install
	rm -rf $(OPENSSLDIR)

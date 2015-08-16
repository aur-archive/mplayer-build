# Maintainer: Iru Dog <mytbk920423@gmail.com>

# NOTE: This package relies on Mplayer's configure script to detect what is
# useful on your system. Build it on the machine you will use it on.
# If some features don't work, install the related optdepends *before*
# building.
# Before reporting any bug, please download a fresh copy of the package from
# the AUR and build the package using makepkg (not Yaourt).
#
# This is a multi-package PKGBUILD for testing. If it's not satisfactory, 
# I'll use the original single-package one instead.

pkgbase=mplayer-build
pkgname=mplayer-build
true && pkgname=("ffmpeg-lite" "mplayer-build")
arch=('i686' 'x86_64')
pkgver=1.1.1
pkgrel=1
license=('GPL')
depends=('libxv' 'alsa-lib' 'libass' 'xvidcore' 'libtheora' 'libpng' 'libmad' 'libgl' 'openal' ) 
makedepends=('yasm')

_mplayer_tar='mplayer-export-snapshot.tar.bz2'
_mplayer_source="http://www.mplayerhq.hu/MPlayer/releases/$_mplayer_tar"

_ffmpeg_tar='ffmpeg-snapshot.tar.bz2'
# because of the problems in snapshot ffmpeg, now use stable
#_use_stable=1
ffmpeg_ver=1.2.1snapshot
#_ffmpeg_tar="ffmpeg-${ffmpeg_ver}.tar.bz2"
_ffmpeg_source="http://ffmpeg.org/releases/$_ffmpeg_tar"

source=("${_mplayer_source}"
		"${_ffmpeg_source}")
sha256sums=("SKIP"
			 "SKIP")

prepare() {
	cd $srcdir
	mv mplayer-*/ mplayer/
	mv ffmpeg mplayer/ffmpeg

}

build(){

	if [[ -n $CC ]]; then
		APPEND="--cc=${CC}" #maybe you want to use clang
	fi
	cd mplayer/ffmpeg
	
	echo -e "\033[31m building ffmpeg\033[0m"
	./configure \
		--prefix=/usr \
		--enable-gpl \
		--enable-version3 \
		--enable-shared \
		--enable-libxvid \
		--enable-libvorbis \
		--enable-libtheora \
		--enable-postproc \
		--disable-ffserver \
		--disable-outdev=oss,caca \
		--disable-debug \
		--disable-static \
		${APPEND}

	echo -e "\033[31m start building ffmpeg \033[0m"
	make
	make tools/qt-faststart
}

package_ffmpeg-lite() {
	pkgdesc='a complete, cross-platform solution to record, convert and stream audio and video'
	true && pkgver=${ffmpeg_ver}
	true && pkgrel=1
	url='http://ffmpeg.org/'
	conflicts=('ffmpeg')
	provides=("ffmpeg=$pkgver")
	depends=('sdl' 'zlib' 'libva' 'libvorbis' 'alsa-lib' 'bzip2')

	cd $srcdir/mplayer/ffmpeg/
	make DESTDIR=$pkgdir install
	install -D -m755 tools/qt-faststart "$pkgdir/usr/bin/qt-faststart"
}

package_mplayer-build() {
	pkgdesc='Famous multimedia player,snapshot version, without mencoder and GUI'
	url='http://www.mplayerhq.hu/'
	conflicts=('mplayer')
	provides=("mplayer")
	backup=('etc/mplayer/codecs.conf' 'etc/mplayer/input.conf')
	cd $srcdir/mplayer/

	unset CFLAGS CXXFLAGS LDFLAGS 
	./configure \
		--prefix=/usr --confdir=/etc/mplayer --enable-menu \
		--disable-mencoder --disable-mpg123 --disable-x264 --disable-static \
		--disable-aa --disable-caca --disable-md5sum --disable-esd \
		--disable-ossaudio --disable-arts --disable-cddb --disable-libdv \
		--disable-lirc --disable-lircc --disable-libcdio \
		--disable-libgsm --disable-librtmp --disable-libdca --disable-speex \
		--disable-libopencore_amrnb --disable-libopencore_amrwb \
		--disable-pulse --disable-jack --disable-xss \
		--disable-libschroedinger-lavc --disable-faad \
		--disable-faac --disable-vdpau 

	echo -e "\033[31m start building mplayer \033[0m"
	make
	make DESTDIR=$pkgdir install
}


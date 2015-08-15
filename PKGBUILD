# Contributor: Carlos Sanchez <cargabsj175@vegnux.org.ve>
# Maintainer for Parabola GNU/Linux: Omar Botta <omarbotta@gnulinuxlibre.net>
# Contributor: Frederic Bezies <fredbezies@gmail.com>
#
# Based on work made by Carlos Sanchez
#
pkgname=gnash-trunk-git
pkgver=0.8.11.r22195.g7349e86
_gitname=gnash
pkgrel=1
pkgdesc="The GNU SWF Player based on GameSWF - git development version"
arch=('i686' 'x86_64')
url="http://www.gnu.org/software/gnash/"
license=('GPL3')
depends=('curl' 'giflib' 'sdl' 'libjpeg' 'libpng' 'libltdl' 'libgl' 
'glu' 'speex' 'fontconfig' 'cairo' 'ffmpeg' 'jemalloc' 'boost-libs' 
'gtk2' 'libldap' 'hicolor-icon-theme' 'desktop-file-utils' 'gconf' 
'gtkglext' 'agg')
makedepends=('mesa' 'xulrunner' 'pkgconfig' 'boost' 'git')
provides=(gnash-common gnash-gtk)
conflicts=(gnash-common gnash-gtk gnash-git)
replaces=(gnash-common gnash-gtk)
options=(!emptydirs)
install=gnash.install
backup=(etc/gnashpluginrc)
source=('git://git.sv.gnu.org/gnash.git')
sha256sums=('SKIP')

pkgver() {
	cd $_gitname
	echo "0.8.11.r$(git rev-list --count master).g$(git log -1 --format="%h")" 
}

prepare() {
	cd $_gitname

        chmod +x autogen.sh
	./autogen.sh

}

build() {
	cd $_gitname

	
    ./configure \
    	--prefix=/usr \
    	--sysconfdir=/etc \
    	--with-plugins-install=system \
    	--with-npapi-plugindir=/usr/lib/mozilla/plugins \
    	--enable-gui=sdl,gtk \
    	--enable-media=ffmpeg \
    	--enable-renderer=all \
    	--enable-device=x11

	sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0 /g' -e 's/    if test "$export_dynamic" = yes && test -n "$export_dynamic_flag_spec"; then/      func_append compile_command " -Wl,-O1,--as-needed"\n      func_append finalize_command " -Wl,-O1,--as-needed"\n\0/' libtool

	make

}

package() {
  cd $_gitname
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="$pkgdir" install install-plugin

  install -m755 -d "$pkgdir/usr/share/gconf/schemas"
  gconf-merge-schema "$pkgdir/usr/share/gconf/schemas/gnash.schemas" --domain gnash \
    "$pkgdir"/usr/share/applications/*.schemas
  rm -f "$pkgdir"/usr/share/applications/*.schemas
}


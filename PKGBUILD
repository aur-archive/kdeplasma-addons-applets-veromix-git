# Maintainer: M0Rf30 < morf3089 at gmail dot com >

pkgname=kdeplasma-addons-applets-veromix-git
pkgver=20120608
pkgrel=1
pkgdesc="A plasmoid mixer for the Pulseaudio sound server"
url="http://code.google.com/p/veromix-plasmoid/"
license=('GPL3')
arch=('i686' 'x86_64')
depends=('kdebindings-python' 'kdebase-workspace' 'python2-qt' 'pulseaudio' 'pyxdg' 'python2-dbus')
conflicts=('kdeplasma-addons-applets-veromix' 'kdeplasma-addons-applets-veromix-svn' 'veromix-git')
makedepends=('git' 'kdesdk-scripts')
optdepends=('pulseaudio-equalizer'
            'swh-plugins: equalizer and other effects support')
_gitroot="https://code.google.com/p/veromix-plasmoid/"
_gitname="veromix-plasmoid"
source=('preset.patch')

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone -l "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  
	
	
	# Updating Italian translation...
	cd $srcdir/$_gitname-build/plasma/contents/locale/it/LC_MESSAGES
	wget https://raw.github.com/M0Rf30/translations/master/veromix-plasmoid.po
	# ...and generating .mo files
	cd $srcdir/$_gitname-build
       	
	# Some fixes
	sed 's#/usr/share/kde4/apps#/usr/share/apps#g' -i $srcdir/$_gitname-build/Makefile
	patch -Np1 -i ../preset.patch
      
	make DESTDIR=${pkgdir} install-plasmoid
	 
	# For KDE
        rm ${pkgdir}/usr/share/apps/plasma/plasmoids/veromix-plasmoid/contents/icons
        rm ${pkgdir}/usr/share/apps/plasma/plasmoids/veromix-plasmoid/contents/code/veromixcommon
        rm ${pkgdir}/usr/share/kde4/services/plasma-widget-veromix.desktop
        ln -s -r /usr/share/icons ${pkgdir}/usr/share/apps/plasma/plasmoids/veromix-plasmoid/contents/
        ln -s -r /usr/share/veromix/common ${pkgdir}/usr/share/apps/plasma/plasmoids/veromix-plasmoid/contents/code/veromixcommon
        ln -s -r /usr/share/apps/plasma/plasmoids/veromix-plasmoid/metadata.desktop $pkgdir/usr/share/kde4/services/plasma-widget-veromix.desktop       
        
        # Generating bytecode
	cd ${pkgdir}
        python2 -m compileall .
}

md5sums=('011dbf37f22eb5c37775c36f88f935f1')

# Maintainer: megadriver <megadriver at gmx dot com>
# Based on lxdm from [community]

pkgname=lxdm-light
_pkgname=lxdm
pkgver=0.4.1
pkgrel=4
pkgdesc="Lightweight Display Manager (*kit free!)"
arch=('i686' 'x86_64')
url="http://sourceforge.net/projects/lxdm/"
license=('GPL')
depends=('gtk2' 'xorg-server')
makedepends=('intltool')
conflicts=('lxdm' 'consolekit' 'polkit')
provides=('lxdm')
install=$pkgname.install
backup=('etc/lxdm/lxdm.conf' 'etc/pam.d/lxdm' 'etc/lxdm/Xsession'
        'etc/lxdm/PreLogin' 'etc/lxdm/LoginReady' 'etc/lxdm/PostLogin'
        'etc/lxdm/PostLogout' 'etc/lxdm/PreReboot' 'etc/lxdm/PreShutdown')
source=(http://downloads.sourceforge.net/lxde/$_pkgname-$pkgver.tar.gz
        glib2-2.32.0.patch lxdm.patch lxdm.conf.patch Xsession.patch
        greeter-session.patch lxdm-daemon lxdm-pam)
md5sums=('8da1cfc2be6dc9217c85a7cf51e1e821'
         'a1e3c46a8bef691bc544028f5b6cfe22'
         'baed9055e8825a5511712bc095197519'
         'c50dd01b715b0a236407d48066191601'
         'd2e4a4a22ee2aa1a986be154c647b6c6'
         '28475239d0c8b4fd778ec49f5ec72962'
         '705f394052fdd0dec22e95321d170de0'
         'b20fe3c8487a039050986d60e45233a9')

build() {
  cd $srcdir/$_pkgname-$pkgver

  patch -Np1 -i $srcdir/glib2-2.32.0.patch
  patch -Np1 -i $srcdir/greeter-session.patch
  ./configure --sysconfdir=/etc --prefix=/usr --libexecdir=/usr/lib/lxdm
  make
  	
  patch -Np0 -i $srcdir/lxdm.patch
  patch -Np0 -i $srcdir/lxdm.conf.patch
  patch -Np0 -i $srcdir/Xsession.patch
}

package() {
  cd $srcdir/$_pkgname-$pkgver
  make DESTDIR="$pkgdir" install

  install -m644 $srcdir/lxdm-pam $pkgdir/etc/pam.d/lxdm
  install -Dm755 $srcdir/lxdm-daemon $pkgdir/etc/rc.d/lxdm
  install -d $pkgdir/var/{lib,run}/lxdm

  # fix the greeter location
  sed -i -e "s/local\/libexec/lib\/lxdm/" $pkgdir/etc/lxdm/lxdm.conf

  # avoid conflict with filesystem>=2012.06
  rm -r "$pkgdir/var/run"
}

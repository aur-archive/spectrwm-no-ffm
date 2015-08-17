# Contributor: megadriver <megadriver at gmx dot com>
# Based on spectrwm from [community]
# Inspired by Spectrwm-fork: http://github.com/piffio/Spectrwm-fork
# Contributor: Lex Black <autumn-wind at web dot de>

pkgname=spectrwm-no-ffm
_pkgname=spectrwm
pkgver=1.1.1
pkgrel=1
pkgdesc='Spectrwm without that annoying "focus follows mouse" thing'
arch=('i686' 'x86_64')
url="http://www.spectrwm.org"
license=('custom:ISC')
depends=('dmenu' 'libxrandr' 'libxtst' 'xorg-fonts-misc')
replaces=('scrotwm')
makedepends=('libxt')
optdepends=('scrot: screenshots' 'xlockmore: screenlocking')
backup=(etc/spectrwm.conf)
conflicts=('spectrwm')
provides=('spectrwm')
source=(http://opensource.conformal.com/snapshots/$_pkgname/$_pkgname-$pkgver.tgz \
	LICENSE baraction.sh spectrwm-no-ffm.patch)
md5sums=('fd97b626ea09c3ab302fc9082efe8c31'
         'a67cfe51079481e5b0eab1ad371379e3'
         '950d663692e1da56e0ac864c6c3ed80e'
         'de37dd1e999bd6a3c78e83d915f71715')

build() {
  cd "$srcdir/$_pkgname-$pkgver"
  
  # it is like a patch, only less fragile
  sed -i 's|\"/usr/local/lib/libswmhack.so\"|\"libswmhack.so\"|' spectrwm.c
  sed -i 's/verbose_layout = 0;/verbose_layout = 1;/' spectrwm.c
  sed -i 's/# modkey = Mod1/modkey = Mod4/' spectrwm.conf
  sed -i 's/-\*-terminus-medium-\*-\*-\*-\*-\*-\*-\*-\*-\*-\*-\*/-*-fixed-medium-r-normal-*-14-*-*-*-*-*-iso10646-*/' spectrwm.conf

  # Don't steal my focus, you filthy rat!
  patch -Np0 -i $srcdir/spectrwm-no-ffm.patch

  cd linux
  make PREFIX="/usr"
}

package() {
  cd "$srcdir/$_pkgname-$pkgver/linux"
  make PREFIX="/usr" DESTDIR="$pkgdir" install
  install -Dm644 spectrwm.desktop "$pkgdir/usr/share/xsessions/spectrwm.desktop"
  cd ..
  install -Dm644 spectrwm.conf "$pkgdir/etc/spectrwm.conf"
  install -Dm755 screenshot.sh "$pkgdir/usr/share/spectrwm/screenshot.sh"
  mkdir -p "$pkgdir/etc/spectrwm"
  cp spectrwm_*.conf "$pkgdir/etc/spectrwm/"
  cd "$srcdir"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm755 baraction.sh "$pkgdir/usr/share/spectrwm/baraction.sh"

  ln -s /usr/lib/libswmhack.so.0.0 "$pkgdir/usr/lib/libswmhack.so.0"
  ln -s /usr/lib/libswmhack.so.0.0 "$pkgdir/usr/lib/libswmhack.so"

  # fix this for real in the makefile
  rm "$pkgdir/usr/bin/scrotwm"
  ln -s "/usr/bin/spectrwm" "$pkgdir/usr/bin/scrotwm"
  mkdir -p "$pkgdir"/usr/share/man/{es,it,pt,ru}/man1/
  mv "$pkgdir/usr/share/man/man1/spectrwm_es.1" "$pkgdir/usr/share/man/es/man1/"
  mv "$pkgdir/usr/share/man/man1/spectrwm_it.1" "$pkgdir/usr/share/man/it/man1/"
  mv "$pkgdir/usr/share/man/man1/spectrwm_pt.1" "$pkgdir/usr/share/man/pt/man1/"
  mv "$pkgdir/usr/share/man/man1/spectrwm_ru.1" "$pkgdir/usr/share/man/ru/man1/"
}

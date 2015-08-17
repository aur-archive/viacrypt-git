# Maintainer: Andy Weidenbaum <archbaum@gmail.com>

pkgname=viacrypt-git
pkgver=20140818
pkgrel=1
pkgdesc="One-time-read messaging system"
arch=('i686' 'x86_64')
depends=('mongodb' 'nodejs' 'nodejs-handlebars-xgettext-git' 'nodejs-po2json' 'redis')
makedepends=('git' 'nodejs-bower' 'nodejs-grunt-cli' 'npm')
url="https://viacry.pt"
license=('MIT')
source=(git+https://github.com/vialink/viacrypt
        viacrypt.service)
sha256sums=('SKIP'
            'd5ca28cc0b9b771f605a8b6e5b34f9265de1a00e3f38210039bac4345f1938c1')
provides=('viacrypt')
conflicts=('viacrypt')
options=('!strip')
install=viacrypt.install

pkgver() {
  cd ${pkgname%-git}
  git log -1 --format="%cd" --date=short | sed "s|-||g"
}

build() {
  cd ${pkgname%-git}

  msg 'Installing NPM dependencies...'
  npm install --python=python2

  msg 'Building...'
  grunt
}

package() {
  cd ${pkgname%-git}

  msg 'Installing license...'
  install -Dm 644 COPYING "$pkgdir/usr/share/licenses/viacrypt/COPYING"

  msg 'Installing documentation...'
  install -Dm 644 README.md "$pkgdir/usr/share/doc/viacrypt/README.md"

  msg 'Installing appdirs...'
  install -dm 755 "$pkgdir/usr/share/viacrypt"
  for _appdir in assets bin bower_components config locale node_modules src static template; do
    cp -dpr --no-preserve=ownership $_appdir "$pkgdir/usr/share/viacrypt/$_appdir"
  done

  msg 'Installing appfiles...'
  for _appfile in bower.json Gruntfile.js package.json; do
    install -Dm 644 $_appfile "$pkgdir/usr/share/viacrypt/$_appfile"
  done

  msg 'Installing executables...'
  for _bin in xgettext.sh; do
    install -Dm 755 $_bin "$pkgdir/usr/share/viacrypt/$_bin"
  done

  msg 'Installing viacrypt.service'
  install -Dm 644 "$srcdir/viacrypt.service" "$pkgdir/usr/lib/systemd/system/viacrypt.service"

  msg 'Cleaning up pkgdir...'
  find "$pkgdir" -type f -name .gitignore -exec rm -r '{}' +
  find "$pkgdir" -type d -name .git -exec rm -r '{}' +
}

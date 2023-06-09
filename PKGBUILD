# Maintainer: Johannes Schindelin <johannes.schindelin@gmx.de>

_realname=wintoast
pkgname=("${_realname}")
pkgver=1.0.0.275.a44fa02
pkgrel=1
pkgdesc="A console application to show a toast notification on Windows 8 and 10 (msys2)"
arch=('any')
url="https://github.com/mohabouje/WinToast"
license=('MIT')

options=('!strip')

source=("${_realname}"::"git+https://github.com/git-for-windows/WinToast#tag=g4w-20221103-01")

sha256sums=('SKIP')

die () {
  echo "$*" >&2
  exit 1
}

case "$CARCH" in
i686) WARCH=x86; OUTDIR=Release;;
x86_64) WARCH=x64; OUTDIR=x64/Release;;
aarch64) WARCH=ARM64; OUTDIR=ARM64/Release;;
*) die "Unsupported architecture: $CARCH"
esac

pkgver() {
  cd "$srcdir/wintoast"
  echo 1.0.0.$(git rev-list --count HEAD).$(git rev-parse --short HEAD)
}

build() {
  cd "$srcdir/wintoast"

  (cd example/console-example/ &&
   cmd //c '..\..\..\..\msbuild\hMSBuild.bat -notamd64 -vsw-version local /p:Configuration=Release /p:Platform='"$WARCH")

  cp example/console-example/$OUTDIR/WinToast\ Console\ Example.exe wintoast.exe

  test -z "$SIGNTOOL" ||
  eval "$SIGNTOOL wintoast.exe"
}

package () {
  cd "$srcdir"/wintoast

  install -d "${pkgdir}/${MSYSTEM_PREFIX}/bin"
  install -m755 wintoast.exe "${pkgdir}/${MSYSTEM_PREFIX}/bin"
}

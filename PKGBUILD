# Maintainer: Padfoot <padfoot at exemail dot com dot au>

pkgname=empcommand-svn
pkgver=8044
pkgrel=1
pkgdesc="Multitouch Missile Command."
arch=('any')
url="https://www.libavg.de/site/projects/libavg/wiki/EMPCommand"
license=('GPL3')
depends=('python2'
         'libavg'
         'hicolor-icon-theme')
makedepends=('subversion'
             'gtk-update-icon-cache')
install='empcommand.install'
source=("empcommand.desktop"
        "libavg.logger-fix.patch")
md5sums=('b09dd93c299cfba2a0f6d559b3e7a0ba'
         '7660c7683f875acfe781f619676a1146')

_svntrunk=https://www.libavg.de/svn/trunk/avg_media/mtc/emp_command
_svnmod=emp_command

build() {
    cd "$srcdir"
    msg "Connecting to SVN server..."

    if [[ -d "$_svnmod/.svn" ]]; then
        (cd "$_svnmod" && svn up -r "$pkgver")
    else
        svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
    fi

    msg "SVN checkout done or server timeout"
  
    rm -rf "$srcdir/$_svnmod-build"
    cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
    cd "$srcdir/$_svnmod-build"

    patch -p1 -i $srcdir/libavg.logger-fix.patch
    
    sed -i "s/python/python2/g" scripts/empcommand
    sed -i "s/python/python2/g" empcommand/*.py
}

package() {
    _python_libs=`python2 -c "from distutils import sysconfig; print sysconfig.get_python_lib()"`
    
    mkdir -p \
        "${pkgdir}${_python_libs}" \
        "$pkgdir/usr/bin" \
        "$pkgdir/usr/share/icons/hicolor/scalable/apps" \
        "$pkgdir/usr/share/applications" || return 1

    cd "$srcdir/$_svnmod-build"

    cp -r empcommand "${pkgdir}${_python_libs}"
    cp empcommand/media/logo.png "$pkgdir/usr/share/icons/hicolor/scalable/apps/empcommand.png"
    
    cd ./scripts
    install -D -m755 empcommand "$pkgdir/usr/bin"
    
    cd "$srcdir"
    install -D -m755 empcommand.desktop "$pkgdir/usr/share/applications"
}

# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

_linuxprefix=linux-xanmod-lts
_extramodules=$(find /usr/lib/modules -type d -iname 5.15.89*xanmod* | rev | cut -d "/" -f1 | rev)

pkgname="$_linuxprefix-zfs"
pkgver=2.1.7
pkgrel=515891
pkgdesc='Kernel modules for the Zettabyte File System.'
arch=('x86_64')
url="http://zfsonlinux.org/"
license=('CDDL')
groups=("$_linuxprefix-extramodules")
depends=("$_linuxprefix" "kmod" "zfs-utils=${pkgver}")
makedepends=("$_linuxprefix-headers")
provides=("zfs=${pkgver}")
install=zfs.install
source=("https://github.com/zfsonlinux/zfs/releases/download/zfs-${pkgver}/zfs-${pkgver}.tar.gz"{,.asc})
sha256sums=(SKIP)
            'SKIP')



prepare() {
    cd "zfs-${pkgver}"
    ./autogen.sh
    sed -i "s|\$(uname -r)|${_kernver}|g" configure
}

build() {
    _kernver=$(find /usr/lib/modules -type d -iname 5.15.89*xanmod* | rev | cut -d "/" -f1 | rev)

    cd "zfs-${pkgver}"
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --libdir=/usr/lib \
                --datadir=/usr/share --includedir=/usr/include --with-udevdir=/lib/udev \
                --libexecdir=/usr/lib/zfs-${pkgver} --with-config=kernel \
                --with-linux=/usr/lib/modules/${_kernver}/build
    make
}

package(){
    cd "zfs-${pkgver}"
    install -Dm644 module/*/*.ko -t "$pkgdir/usr/lib/modules/$_extramodules/"

    # compress each module individually
    find "$pkgdir" -name '*.ko' -exec xz -T1 {} +
}

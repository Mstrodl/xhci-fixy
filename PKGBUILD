# Maintainer: Mary Strodl <digimend-xhci@coolmathgames.tech>

pkgname=digimend-xhci
KERNEL_MAJOR=5
pkgver=${KERNEL_MAJOR}.5.5
pkgrel=1

pkgdesc="Fix for Huion 1060P tablets (note: tracks linux package. probably won't work on any other versions)"
arch=('any')
url='https://digimend.github.io'
license=('GPL2')

depends=('dkms')
makedepends=()

source=("https://cdn.kernel.org/pub/linux/kernel/v${KERNEL_MAJOR}.x/linux-${pkgver}.tar.xz"
	"https://cdn.kernel.org/pub/linux/kernel/v${KERNEL_MAJOR}.x/linux-${pkgver}.tar.sign"
	"dkms.conf"
	"digimend_xhci.patch"
	"move_misc.patch")
noextract=("linux-$pkgver.tar.xz")
# (Stolen from arch's linux package)
validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
  '8218F88849AAC522E94CF470A5E9288C4FA415FA'  # Jan Alexander Steffens (heftig)
)

prepare() {
  # Only extract the files we need, otherwise it's massive
  tar -xvJ linux-$pkgver.tar.xz linux-$pkgver/drivers/usb/host linux-$pkgver/drivers/usb/misc/usb_u132.h
}

package() {
  # Copy dkms build conf
  mkdir -p "$pkgdir"/usr/src/"$pkgname"-"$pkgver"
  cp dkms.conf "$pkgdir"/usr/src/"$pkgname"-"$pkgver"

  # Apply patches
  cd linux-"$pkgver"/drivers/usb
  patch -ruN -d host < ../../digimend_xhci.patch
  patch -ruN -d host < ../../move_misc.patch

  # Copy over the source
  cp -rf ./host/* ./misc/usb_u132.h "$pkgdir"/usr/src/"$pkgname"-"$pkgver"

  # Set version
  sed "s/PACKAGE_VERSION=.*/PACKAGE_VERSION=\"$pkgver\"/g" \
      -i "$pkgdir"/usr/src/"$pkgname"-"$pkgver"/dkms.conf
}

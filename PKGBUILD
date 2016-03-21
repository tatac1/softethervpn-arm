# Maintainer: ava1ar <mail(at)ava1ar(dot)me>

pkgname=softethervpn
pkgver=v4.19.9605.beta
pkgrel=1
pkgdesc="Multi-protocol VPN software from University of Tsukuba"
url="http://www.softether.org/"
arch=('armv7h')
source=('http://jp.softether-download.com/files/softether/v4.19-9605-beta-2016.03.06-tree/Linux/SoftEther_VPN_Server/32bit_-_ARM_EABI/softether-vpnserver-v4.19-9605-beta-2016.03.06-linux-arm_eabi-32bit.tar.gz'
        'softethervpn-server.service'
        )
sha1sums=('38f32c87624bbf7e8205760a7ffb2e1d35485ae3'
          '06cd320553daf0dffdf6a81a22d630fbe211fc33'
          )
license=('GPL2')
depends=('bash' 'openssl' 'zlib')
makedepends=()

#prepare() {
#  # clean existing sources if any
#  rm -rf "${srcdir}"/SoftEtherVPN
#
#  # cloning only last commit of master branch, since complete repository is pretty heavy
#  git clone https://github.com/SoftEtherVPN/SoftEtherVPN.git --single-branch --depth 1
#}
#
#pkgver() {
#  cd "${srcdir}"/SoftEtherVPN
#  git log | grep -o -m1 'v[0-9].*' | tr '-' '.'
#}

build() {
  cd "${srcdir}/vpnserver"

  make i_read_and_agree_the_license_agreement
}

package(){
  cd "${srcdir}/vpnserver"

  install -Dm644 hamcore.se2 "${pkgdir}"/usr/lib/softethervpn/hamcore.se2
  install -d "${pkgdir}"/usr/bin

  for inst in vpnserver vpncmd
  do
    install -Dm755 ${inst} "${pkgdir}"/usr/lib/softethervpn/${inst}/${inst}
    ln -s /usr/lib/softethervpn/hamcore.se2 "${pkgdir}"/usr/lib/softethervpn/${inst}/hamcore.se2
    echo "#!/bin/sh" > "${pkgdir}"/usr/bin/${inst}
    echo /usr/lib/softethervpn/${inst}/${inst} '"$@"' >> "${pkgdir}"/usr/bin/${inst}
    echo 'exit $?' >> "${pkgdir}"/usr/bin/${inst}
    chmod 755 "${pkgdir}"/usr/bin/${inst}
  done

  install -d "${pkgdir}"/usr/lib/systemd/system
  install -Dm644 "${srcdir}"/*.service "${pkgdir}"/usr/lib/systemd/system
}

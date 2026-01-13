# Maintainer: willemw <willemw12@gmail.com>

pkgname=termusic-git
pkgver=r3925.8326cbed
pkgrel=1
pkgdesc='Music Player TUI written in Rust'
arch=(x86_64)
url=https://github.com/sekiryl/termusic
license=(GPL-3.0-or-later MIT)
depends=(
  dbus gst-libav gst-plugins-bad gst-plugins-base gst-plugins-good gst-plugins-ugly gstreamer
  libsixel mpv opus ueberzugpp) # alsa-lib libmpv.so
makedepends=(cargo clang git protobuf soundtouch)
optdepends=(
  'emoji-font: display emojis'
  'ffmpeg: extract audio by downloader'
  'ueberzug: cover art'
  'yt-dlp: download files')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
options=(!lto)
source=("$pkgname::git+$url.git")
sha256sums=('SKIP')

pkgver() {
  cd "$pkgname"
  printf "r%s.%s" \
    "$(git rev-list --count HEAD)" \
    "$(git rev-parse --short HEAD)"
}

prepare() {
  export RUSTUP_TOOLCHAIN=stable
  cd $pkgname
  #cargo update
  cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
  export RUSTUP_TOOLCHAIN=stable CARGO_TARGET_DIR=target
  cd $pkgname
  cargo build --frozen --release --features rusty-soundtouch -j2
}

package() {
  install -Dm755 "$pkgname/target/release/${pkgname%-git}"{,-server} -t "$pkgdir/usr/bin"
  install -Dm644 $pkgname/LICENSE_MIT "$pkgdir/usr/share/licenses/${pkgname%-git}/LICENSE"
}

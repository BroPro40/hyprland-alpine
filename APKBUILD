# Maintainer: bropro40@github.com
pkgname=hyprland
pkgver=0.29.0
pkgrel=0
pkgdesc="Highly customizable dynamic tiling Wayland compositor"
url="https://github.com/hyprwm/Hyprland"
arch="all"
license="BSD-3-Clause AND MIT"
makedepends="
	cairo-dev
	cmake
	eudev-dev
	glslang-dev
	hwdata-dev
	jq
	libcap-dev
	libdisplay-info-dev
	libinput-dev
	libliftoff-dev
	libseat-dev
	libxcb-dev
	libxkbcommon-dev
	mesa-dev
	meson
	pango-dev
	pixman-dev
	vulkan-loader-dev
	wayland-dev
	wayland-protocols
	xcb-util-image-dev
	xcb-util-renderutil-dev
	xcb-util-wm-dev
	xkeyboard-config-dev
	xwayland-dev
	"
subpackages="$pkgname-doc $pkgname-dev"
source="$pkgname-$pkgver.tar.gz::https://github.com/hyprwm/Hyprland/releases/download/v$pkgver/source-v$pkgver.tar.gz
	wlroots.patch
	gcc13.patch
	no-werror.patch
	"
builddir="$srcdir/hyprland-source"
options="!check" # doesn't have any checks

build() {
	export CFLAGS="$CFLAGS -O2 -DNDEBUG"
	export CXXFLAGS="$CXXFLAGS -O2 -DNDEBUG"
	abuild-meson \
		-Dsystemd=disabled \
		-Dxwayland=enabled \
		. output
	meson compile -C output
}

package() {
	DESTDIR="$pkgdir" meson install --no-rebuild -C output

	# remove wlroots files
	rm -r "$pkgdir"/usr/include/wlr/
	rm -r "$pkgdir"/usr/lib/pkgconfig/
	rm -r "$pkgdir"/usr/lib/libwlroots.a

	mv "$pkgdir"/usr/share/pkgconfig "$pkgdir"/usr/lib/pkgconfig
}

dev() {
	default_dev
	amove usr/share/hyprland-protocols/
}

sha512sums="
26008da98049f85f14be821d73cc42f4ec3e59669375ff78e633d187b15abf1eeb51e5f25d8a12d3ddde14de8d83aa3fbed23a9713bf086b30620f0a93320ce5  hyprland-0.29.0.tar.gz
# 24ce00c9c846c1899b399bf18627ba79d815d00a504ae9d0921b2f8a2fcf4225e1d73e4e558aba8bb3a83e46c705abf12c93a8052b6ecbe535f652b7b4c7b3f5  execinfo.patch
0df8586ce1d87655ed8f14281e207d768eb124c9af6544c66dccedc41a442365eb9c9d871bf5c9757e8f7332057e4da582cdcff24410f7c6823a619a10095104  wlroots.patch
787192542942a796515c2457f6363b372ffff82ec39745a66cf5389660f6bf3fcf61135c836ea0e7dce0e2886994410bbd6b3e634f3616ac34d60009b9f1a1a4  gcc13.patch
1185089d35c4bbb28e56a8d9594e37f0ffa13c924dc56db0dc54f5f3b25999472df411fa68006c1b2fd5760f91484a1104beda005c807ece42aa02e323266e6c  no-werror.patch
"
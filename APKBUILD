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
	execinfo.patch
	gcc13.patch
	no-werror.patch
	git.patch
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
c31844a72274e8a9c0c5f026c401fe4941e450c04c441fde5dac140428eb92418fb5dd7a1e82d979f26da4294f07630d011c2dc38c5b98812c779ab6f35a6409 execinfo.patch
a82391408f1d86c440400e80ea4b66a9376851cbb4425b748ab272f5227abfed1a74c6f0fdc26a12b9288bd9493b94cf78891bbbbfd816a0079b2c9037d291d9  gcc13.patch
042ff7d323b0d16feca10e45dbf0b1eac41daea1f03e8af40d205da35fafa8b8fd0ae21a1dcdea924ad6c39a68bd8ee72ebde014192b7607e983c3d3e66be76a  no-werror.patch
8c1cad916bcbbf75b3d81d0dec3224164b8ab53be247c6982a053e8160b08be05072bf520fc011590973f5d92d329b597a852243d4bb8a84907e9af31214f6c2  git.patch
"
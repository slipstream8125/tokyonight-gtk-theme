# Maintainer: Slipstream8125 <slipstream8125@proton.me>

pkgname="tokyonight-gtk-theme"
pkgver=r105.12cbfba1
pkgrel=1
pkgdesc="A GTK theme based on the Tokyo Night colour palette"
arch=("any")
url="https://github.com/StratOS-Linux/${pkgname}"
license=("GPL3")
depends=("gnome-themes-extra")
optdepends=("gtk-engine-murrine: for GTK2/Murrine engine support")
makedepends=("git" "sassc")
options=(!strip !debug)
# source=("git+${url}.git#branch=master")
source=()
# sha256sums=('SKIP')  # using git source; if using a stable release, replace with real sum
# Customisation variables
accent_variants=("default")
color_variants=(
    "dark"
    # "light"
)
size_variant="standard"
tweaks_list=(
    #"black" 
    "macos"
    # "float"
    #"outline"
)
theme_name="$pkgname"

# install=tn.install

prepare() {
  cp -ra $startdir/TokyoNight $srcdir/
  cp $startdir/LICENSE $srcdir/
  cp $startdir/README.md $srcdir/
}

pkgver() {
  cd "${srcdir}"
  echo "r$(git rev-list --count HEAD).$(git rev-parse --short HEAD)"
}

package() {
  # Documentation & license
  install -D -m 0644 "${srcdir}/README.md" "${pkgdir}/usr/share/doc/${pkgname}/README.md"
  install -D -m 0644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # Themes: use upstream install.sh
  theme_dest="${pkgdir}/usr/share/themes"
  mkdir -p "${theme_dest}"

  # Build the arguments for variants
  theme_variant_args=()
  for t in "${accent_variants[@]}"; do
    theme_variant_args+=( "-t" "${t}" )
  done
  for c in "${color_variants[@]}"; do
    theme_variant_args+=( "-c" "${c}" )
  done
  theme_variant_args+=( "-s" "${size_variant}" )

  tweaks_arg=()
  if [ ${#tweaks_list[@]} -gt 0 ]; then
    tweaks_arg+=( "--tweaks" "${tweaks_list[*]}" )
  fi

  # Call install.sh with correct flags
  cd "${srcdir}/TokyoNight/themes/"
  ./install.sh \
    --dest "${theme_dest}" \
    --name "${theme_name}" \
    "${theme_variant_args[@]}" \
    "${tweaks_arg[@]}" \
    -l

  # Icons
  icon_dest="${pkgdir}/usr/share/icons"
  mkdir -p "${icon_dest}"
  # Ensure we handle the case where icons/ may have non‑theme files
  for icondir in icons/*; do
    if [ -d "${icondir}" ]; then
      cp -ra "${icondir}" "${icon_dest}/"
    fi
  done
  # Copy manually
  cp -ra "${srcdir}/TokyoNight/icons/Tokyonight-Dark" "${pkgdir}/usr/share/icons/"
  cp -ra "${srcdir}/TokyoNight/icons/Tokyonight-Moon" "${pkgdir}/usr/share/icons/"
}

# vim:set ts=2 sw=2 et:

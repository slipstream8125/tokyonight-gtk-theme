# Maintainer: Slipstream8125 <slipstream8125@proton.me>

pkgname="tokyonight-gtk-theme"
_reponame="Tokyonight-GTK-Theme"
pkgver=r91.98bd5965
pkgrel=1
pkgdesc="A GTK theme based on the Tokyo Night colour palette"
arch=("any")
url="https://github.com/Fausto-Korpsvart/${_reponame}"
license=("GPL3")
depends=("gnome-themes-extra")
optdepends=("gtk-engine-murrine: for GTK2/Murrine engine support")
makedepends=("git" "sassc")
# source=("git+${url}.git#branch=master")
source=()
sha256sums=('SKIP')  # using git source; if using a stable release, replace with real sum

# Customisation variables
accent_variants=("all")
color_variants=("dark" "light")
size_variant="standard"
tweaks_list=("black" "float" "outline")
theme_name="$pkgname"

pkgver() {
  cd "${srcdir}" #/${_reponame}"
  echo "r$(git rev-list --count HEAD).$(git rev-parse --short HEAD)"
}

prepare() {
  cd "${srcdir}" #/${_reponame}"
  # no extra steps needed
}

package() {
  cd "${srcdir}" #/${_reponame}"

  # Documentation & license
  install -D -m 0644 README.md "${pkgdir}/usr/share/doc/${pkgname}/README.md"
  install -D -m 0644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

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
      cp -r "${icondir}" "${icon_dest}/"
    fi
  done
}

# vim:set ts=2 sw=2 et:

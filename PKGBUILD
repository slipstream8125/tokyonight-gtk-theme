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
source=("git+${url}.git#branch=master")
sha256sums=('SKIP')  # using git source; if using tagged release, replace with real sum

accent_variants=("all")         # e.g. "green", "purple", "grey", "all"
color_variants=("dark" "light")   # light/dark variants
size_variant="standard"           # "standard" or "compact"
tweaks_list=("black" "float" "outline")             # e.g. "macos", "float", "outline", "moon", "storm", etc.
theme_name="$pkgname"             # name to install as

pkgver() {
  cd "${srcdir}/${_reponame}"
  # count commits + short hash
  echo "r$(git rev-list --count HEAD).$(git rev-parse --short HEAD)"
}

prepare() {
  cd "${srcdir}/${_reponame}"
  # No special build step needed; install.sh handles CSS/SASS etc.
}

package() {
  cd "${srcdir}/${_reponame}"

  # Documentation & license
  install -D -m 0644 README.md "${pkgdir}/usr/share/doc/${pkgname}/README.md"
  install -D -m 0644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # Themes: use upstream install.sh
  # Construct install.sh flags
  # -d / --dest => destination dir
  # -n / --name => theme folder name
  # -t / --theme => accent colour variants
  # -c / --color => light/dark
  # -s / --size => size variant
  # --tweaks => tweaks like macos, float, etc.
  # -l / --libadwaita for linking gtk4 theme for libadwaita apps
  theme_dest="${pkgdir}/usr/share/themes"
  mkdir -p "${theme_dest}"
  # Build the theme variant string parameters
  theme_variant_args=()
  for tv in "${accent_variants[@]}"; do
    theme_variant_args+=( "-t" "${tv}" )
  done
  for cv in "${color_variants[@]}"; do
    theme_variant_args+=( "-c" "${cv}" )
  done
  theme_variant_args+=( "-s" "${size_variant}" )

  # Build tweaks string
  tweaks_arg=()
  if [ ${#tweaks_list[@]} -gt 0 ]; then
    # join tweaks with spaces
    tweaks_arg+=( "--tweaks" "${tweaks_list[*]}" )
  fi

  # Now call install.sh
  ./install.sh \
    --dest "${theme_dest}" \
    --name "${theme_name}" \
    "${theme_variant_args[@]}" \
    "${tweaks_arg[@]}" \
    -l

  # Icons: copy icon themes
  icon_dest="${pkgdir}/usr/share/icons"
  mkdir -p "${icon_dest}"
  for icondir in icons/*; do
    if [ -d "${icondir}" ]; then
      cp -r "${icondir}" "${icon_dest}/"
    fi
  done
}

# vim:set ts=2 sw=2 et:

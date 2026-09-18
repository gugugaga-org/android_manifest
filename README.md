# LineageOS 20 RK3399 manifest

This repository is the public `repo` manifest for the RK3399 TV product on
the LineageOS 20 / Android 13 platform. The manifest branch is `lineage-20`.

## Checkout

```sh
repo init -u https://github.com/gugugaga-org/android_manifest \
    -b lineage-20 -m default.xml
repo sync -c -j$(nproc)
```

The manifest keeps the standard Android source paths. Product-specific code
is placed under `device/rockchip/rk3399`, the kernel under
`kernel/rockchip/rk3399`, and Rockchip packaging tools under
`tools/rockchip/rktools`.

## Build

```sh
source build/envsetup.sh
lunch lineage_rk3399_tpm312-userdebug
m
```

No download or overlay scripts are required. The repository is intended to
be used with normal `repo sync`, and board-specific files are versioned in the
projects listed by the manifest.

The product can include the pinned `vendor/gapps_tv` MindTheGapps Android TV
application package when `WITH_GMS=true`. Those APKs are third-party
materials; review their licenses and redistribution terms before shipping an
image. Set `TPM312_WITH_GAPPS=false` (and leave `WITH_GMS` disabled) when
preparing a minimal build.

The current LineageOS 20 tree packages KernelSU Manager as a development-only
application. It does not add kernel-side KernelSU support unless the kernel
project is extended separately.

## Manual image packaging

After a successful build, place the generated images under the standard
`rockdev/Image-lineage_rk3399_tpm312` directory expected by the Rockchip
packaging metadata, then run the packaging tools from
`tools/rockchip/rktools/linux/Linux_Pack_Firmware`. The package manifest is
kept in that project; no public repository script performs the packaging
automatically.

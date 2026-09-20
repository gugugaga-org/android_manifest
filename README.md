# LineageOS 22.2 RK3399 manifest

This repository is the public `repo` manifest for the RK3399 TV product on
the LineageOS 22.2 / Android 15 platform. The manifest branch is
`lineage-22.2`.

## Checkout

```sh
repo init -u https://github.com/gugugaga-org/android_manifest \
    -b lineage-22.2 -m default.xml
repo sync -c -j$(nproc)
```

The manifest keeps the standard Android source paths. Product-specific code
is placed under `device/rockchip/rk3399`, the kernel under
`kernel/rockchip/rk3399`, and the Rockchip packaging tools under `RKTools`.

The projects this product forks are tracked from the organization on their
`lineage-22.2` branches. The verified Android 14 BSP projects that were not
ported (the kernel, the vendor tree and part of the Rockchip HALs) stay on
their `lineage-21.0` branches, and the third-party Rockchip and MindTheGapps
projects are pinned to explicit revisions.

## Build

```sh
source build/envsetup.sh
lunch lineage_rk3399_tpm312-userdebug
m
```

No download or overlay scripts are required. The repository is intended to
be used with normal `repo sync`, and board-specific files are versioned in
the projects listed by the manifest.

The product includes the pinned `vendor/gapps_tv` MindTheGapps Android TV
application package when `WITH_GMS=true`. Those APKs are third-party
materials; review their licenses and redistribution terms before shipping an
image. The only GApps switch is the standard Lineage `WITH_GMS` variable;
set `WITH_GMS=false` when preparing a minimal build.

The LineageOS 22.2 TPM312 product carries the kernel-side KernelSU project
and the official signed KernelSU Manager separately from GApps. The manager
is installed by `vendor/mtgapps`; it does not add a second GApps switch.

## Manual image packaging

After a successful build, place the generated images under the standard
`rockdev/Image-lineage_rk3399_tpm312` directory expected by the Rockchip
packaging metadata, then run the packaging tools from
`RKTools/linux/Linux_Pack_Firmware`. The package manifest is kept in that
project; no public repository script performs the packaging automatically.

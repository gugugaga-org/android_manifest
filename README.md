# TPM312 RK3399 Android 12 manifest

This is the standard Android `repo` manifest for the TPM312 RK3399 Android 12
build. Board changes are committed directly to the project repositories; there
is no overlay or patch application step.

## Checkout

```bash
repo init \
  -u https://github.com/gugugaga-org/android_manifest.git \
  -b aosp-12 \
  -m default.xml
repo sync -c --no-clone-bundle -j"$(nproc)"
```

There are intentionally no setup/build/download scripts in this repository.
After `repo sync`, all source changes and the vendor inputs are already present.

## Project split

The following projects are hosted by `gugugaga-org` and use branch `aosp-12`:

| Checkout path | Repository |
| --- | --- |
| `build/make` | `android_build` |
| `device/asus/tinker_board` | `android_device_asus_tinker_board` |
| `device/rockchip/common` | `android_device_rockchip_common` |
| `device/rockchip/rk3399` | `android_device_rockchip_rk3399` |
| `external/rktoolbox` | `android_external_rktoolbox` |
| `frameworks/av` | `android_frameworks_av` |
| `frameworks/opt/net/wifi` | `android_frameworks_opt_net_wifi` |
| `hardware/rockchip/hw_output` | `android_hardware_rockchip_hw_output` |
| `hardware/rockchip/hwcomposer/drmhwc` | `android_hardware_rockchip_hwcomposer_drmhwc` |
| `hardware/rockchip/libhwjpeg` | `android_hardware_rockchip_libhwjpeg` |
| `kernel/rockchip/rk3399` | `android_kernel_rockchip_rk3399` |
| `kernel/rockchip/rk3399/KernelSU` | `android_kernel_su` |
| `packages/apps/TvSettings` | `android_packages_apps_tvsettings` |
| `u-boot` | `android_u-boot` |
| `vendor/rockchip/common` | `android_vendor_rockchip_common` |
| `vendor/mtgapps` | `android_vendor_mtgapps` |

All other projects stay on the pinned Rockchip Android 12 or AOSP revisions
from `default.xml`. To upgrade a component, update its `aosp-12` branch and
then sync the checkout; no patch regeneration is needed.

The `android_vendor_mtgapps` project is populated from the existing Lineage20
TPM312 vendor tree. It includes the MindTheGapps ATV prebuilts and the
KernelSU Manager APK, so there is no `fetch-blobs` step. To update these
inputs, replace the contents of that project manually and commit the result.

## Build manually

```bash
source build/envsetup.sh
lunch rk3399_tpm312-userdebug

# U-Boot / trust
./build.sh -U -J"$(nproc)"

# Kernel and resource image
cd kernel/rockchip/rk3399
make ARCH=arm64 rockchip_defconfig android-11.config \
    tpm312_kernelsu.config disable_incfs.config
make ARCH=arm64 -j"$(nproc)" rk3399-tpm312.img
cd ../../../u-boot
./scripts/pack_resource.sh ../../../kernel/rockchip/rk3399/resource.img
cp -f resource.img ../../../kernel/rockchip/rk3399/resource.img
cd ..

# Android image
./build.sh -A -J"$(nproc)"
```

For kernel-only changes, copy `kernel/rockchip/rk3399/arch/arm64/boot/Image` and the
generated `rk3399-tpm312.dtb` into the product output before repacking `boot.img`
with the normal Rockchip `mkbootimg` command. Flashing and final image packaging
remain manual because they depend on the Rockchip tools and target hardware.

## Important licensing note

The device project currently contains the ViPER4Android APK and shared
libraries from the previous build input. Check the applicable redistribution
terms and choose the GitHub repository visibility accordingly before publishing
the repositories.

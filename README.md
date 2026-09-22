# TPM312 LineageOS 23.2 manifest

This branch tracks the TPM312 RK3399 Android 16 QPR2 product on LineageOS
23.2. The manifest uses the standard Android source paths and includes the
TPM312 project list in `snippets/tpm312.xml`.

## Checkout

```sh
repo init -u https://github.com/gugugaga-org/android_manifest \
    -b lineage-23.2 --depth=1 --no-clone-bundle
repo sync -c --no-tags --no-clone-bundle
```

Git LFS is required for the Android TV GApps and WebView APKs. After syncing,
confirm those APKs are materialized rather than Git LFS pointer files.

## Build

```sh
source build/envsetup.sh
lunch lineage_rk3399_tpm312-bp4a-userdebug
m superimage
```

The kernel build needs Android Clang `clang-r450784d` in
`prebuilts/clang/host/linux-x86/`. This compiler is a manual prerequisite.
Images built from this tree require device validation before use.

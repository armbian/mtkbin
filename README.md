# Mediatek binaries

## Why?

This repo contains prebuilt binaries provided by Mediatek (or board vendors) for use with their platforms.

The sources for these binaries are not publicly available, but Mediatek (or boards vendors) provide them as compiled
binaries.

This repo is consumed by `armbian/build` framework to build a full boot stack for specific boards; as such it is not
meant for end-users.

# What?

### `libdram`

This is a DRAM initialization library used by U-Boot and ATF to initialize the DRAM memory controller on Mediatek
platforms.
It is provided as `.a` only (thus subject to ABI compatibility) and linked into TF-A/ATF build.

Mediatek publishes binaries in their GitLab repository: https://gitlab.com/mediatek/aiot/rity/libdram-prebuilt.git

Here we have organized them, per-board, for easier use and with a GitHub mirror.

### `libbase`

This is a low-level hardware abstraction library for Mediatek platforms.

It is provided as `.a` only (thus subject to ABI compatibility) and linked into TF-A/ATF build.

Mediatek publishes binaries in their GitLab repository: https://gitlab.com/mediatek/aiot/bsp/libbase-prebuilt.git

Here we have organized them, per-board, for easier use and with a GitHub mirror.

### `Little Kernel (LK)`

The Little Kernel (LK) is a lightweight `fastboot` compatible bootloader used on Mediatek platforms.

Mediatek publishes builds in their GitLab repository: https://gitlab.com/mediatek/aiot/rity/lk-prebuilt.git but only
for their development kits. Board vendors (or Mediatek themselves) provide prebuilt `lk.bin` files for other boards.

The upstream is here: https://github.com/littlekernel/lk - but Mediatek apparently heavily modifies it with no public
source code available.

It plays a similar role to Rockchip's `spl_loader` blob: the SoC loads it from USB OTG (similar to Rockchip's "Markrom"
USB).

The `lk.bin` itself contains all the BLxx bits (thus probably also `libdram` and `libbase`) plus a final payload
that implements `fastboot` protocol over OTG, allowing users to flash FIP images and disk images to eMMC/UFS storage.

It is an essential part of the flashing process for Mediatek boards, but is not required for normal booting.

# Directory organization

The prebuilt binaries are organized in directories per-board, with each directory containing the relevant
`.a` files for `libdram` and `libbase`, and the `lk.bin` file for Little Kernel.

There might be variants for different storage types (eMMC, UFS) or board revisions.

In the case of Little Kernel, there might be more than one variant per board, and armbian/build selects one based
on trial and error.

Look into each board directory for details.
# mtkbin for Radxa NIO-12L

## `libdram`

A simple mirror of https://gitlab.com/mediatek/aiot/rity/libdram-prebuilt.git at commit
`af4ed95803af1249a2605ce205421bc6e17a256a` with the `genio-1200-evk/libdram.a` binary.

Albeit for the Genio 1200 EVK, this binary also works on Radxa NIO-12L.

## `libbase`

A simple mirror of https://gitlab.com/mediatek/aiot/bsp/libbase-prebuilt.git at commit
`52a3aab16d0f7c0c9ec817acf87b1e973965b7de` with the `mt8195/libbase.a` binary, which works for Radxa NIO-12L.

## `Little Kernel (LK)`

Little Kernel for the Radxa NIO-12L is a bit messy.

Collabora provides `lk.bin` binaries at https://gitlab.collabora.com/mediatek/aiot/boot GitLab pipeline artifacts, but:

1) Recent builds don't work - they seem to expect eMMC, not UFS storage
2) Old builds have been archived/deleted and are no longer available

Here we have a mirror of an old working `lk.bin` binary from Collabora (from "sometime around August/September 2025")
that works with UFS storage. Which exact pipeline produced it is unfortunately unknown.

**Update (Nov 2026): 4-GiB Fastboot Flashing Barrier Fix**  
The original Collabora binary contained a 32-bit integer overflow in the Android sparse image handler (`sparse_unsparse` at address `0x210cb0`: `mul w8, w8, w9`), causing LK to silently drop or wrap block offsets when flashing raw images larger than 4 GiB (such as full desktop images > 4 GiB).  
The `lk.bin` binary is patched in-place (10 instructions updated to 64-bit arithmetic: `umull x8, w8, w9`, `add x23, x23, x8`, `add x20, x20, x8`, and `lsr x8, x20, #0xc`). This enables flawless flashing of images up to the 16 TiB UFS LBA addressing limit without data corruption.

Also included is an `lk_radxa.bin` binary provided by Radxa in their Ubuntu image for NIO-12L
`baoshan-classic-desktop-2204-x01-20231005-133-g1200-radxa-nio-12l-ufs-b9`.

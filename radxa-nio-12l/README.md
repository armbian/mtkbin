# mtkbin for Radxa NIO-12L

## `libdram`

A simple mirror of https://gitlab.com/mediatek/aiot/rity/libdram-prebuilt.git at commit
`af4ed95803af1249a2605ce205421bc6e17a256a` with the `genio-1200-evk/libdram.a` binary.

Albeit for the Genio 1200 EVK, this binary also works on Radxa NIO-12L.

## `libbase`

A simple mirror of https://gitlab.com/mediatek/aiot/bsp/libbase-prebuilt.git at commit
`52a3aab16d0f7c0c9ec817acf87b1e973965b7de` with the `mt8195/libbase.a` binary, which works for Radxa NIO-12L.

## `Little Kernel (LK)`

The Little Kernel (LK) binary for Radxa NIO-12L is the official prebuilt target `mt8395-evk-ufs/lk.bin` published by MediaTek in their upstream repository:
https://gitlab.com/mediatek/aiot/rity/lk-prebuilt.git

### Upstream Version History

| Version | Commit | Date | Size (Bytes) | SHA-256 Checksum | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **v23.1** | `c76361b` | 2023-08-14 | 260,768 | `20e250b31b071e468c3982a6b0d7baea9fe272964539892b3ae3eea61f4b26da` | Initial Radxa Ubuntu release |
| **v25.0** | `301e0c9` | 2025-05-20 | 264,968 | `dee68f835d94866819583d25eace422d0f06358b62c4864e1c0b6dff2ebcd9cf` | Previous working UFS DA in Armbian |
| **v26.1** | `12d7773` | 2026-08-19 | 265,032 | `d907e17b683b094e98ed10d73302d1d9c31efdaa50d271ea8de54f0ed57ec55b` | Current release with 4-GiB sparse fix |

### 4-GiB Fastboot Flashing Barrier Fix (v26.1)

In earlier versions (up to v26.0), the Android sparse image handler in Little Kernel (`write_sparse_image`) contained a 32-bit integer multiplication overflow (`mul w5, w2, w6`) when calculating chunk byte offsets. This caused fastboot flashing to fail or silently corrupt data when writing disk images larger than 4 GiB.

In **v26.1**, MediaTek fixed this upstream (internal commit `3b9265771461598882e6690b28017d487931acd5`), replacing the calculation with 64-bit arithmetic (`umull x3, w2, w7`).

The binary in `radxa-nio-12l/lk/lk.bin` is updated to official upstream **v26.1** (`12d77738d052feefe75127d66ab4a65612c59569`), supporting flawless flashing of large OS images directly out of the box.

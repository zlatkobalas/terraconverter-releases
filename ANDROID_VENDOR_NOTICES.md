# Android vendor notices

The Android build pulls most Python dependencies the same way the desktop
build does (see `THIRD_PARTY_NOTICES.md` for the full list and license
texts). A handful of packages have no official Android/Chaquopy wheel and
are bundled differently — this file explains those exceptions, for anyone
inspecting what's shipped inside the APK.

## openlocationcode (Plus Codes)

License: Apache License 2.0 (same as the desktop build).

`openlocationcode` only publishes a source distribution on PyPI, no wheel,
so Chaquopy's `pip` cannot install it directly. The bundled wheel is built
locally from the same official PyPI source — same code, same license, just
packaged as a wheel instead of downloaded as one.

## pydantic-core

License: MIT (same as the desktop build).

`pydantic-core` (a Rust extension `pydantic` v2 requires) has no official
Android build, on PyPI or through Chaquopy (tracked upstream at
[chaquo/chaquopy#1017](https://github.com/chaquo/chaquopy/issues/1017)).

Until that's resolved, the Android build uses an unofficial, community-built
binary from [Goplr/android-pydantic-core](https://github.com/Goplr/android-pydantic-core)
(release `v2.46.5`), verified against the following checksums at the time
it was vendored:

| File (architecture) | SHA256 |
|---|---|
| `...linux_aarch64.android_24_arm64_v8a.whl` | `72bb35e96f1f2b91d154eb7da16fbc82a5bf5b0a1fabb6a781e1294f47d9e1cb` |
| `...linux_x86_64.android_24_x86_64.whl` | `e62086d0af9ea538700c937e99c86369b5e1e63f339a8830afc5762beb35c456` |

This is third-party binary code, not published by PyPI or the pydantic
project — flagged here for transparency. It will be replaced with an
official or self-built wheel once one becomes available.

## Geohash support — not the `pygeohash` package

TerraConverter's Android build does **not** use the `pygeohash` PyPI
package. The only `pygeohash` release with a universal, pure-Python wheel
(`<3.0`) is GPL-3 licensed, which is incompatible with TerraConverter's
closed-source freeware license; the MIT-licensed releases (`>=3.0`) require
a native extension with no Android build, the same problem as
`pydantic-core` above.

Instead, the Android build includes TerraConverter's own implementation of
the public geohash algorithm (Gustavo Niemeyer, 2008), written from the
published algorithm description rather than from `pygeohash`'s source.
It has been numerically verified to produce identical results to the real
`pygeohash` package (desktop build) across a range of test points and
precisions. This is original TerraConverter code, not a third-party
component — included here only so the geohash feature's provenance is
clear.

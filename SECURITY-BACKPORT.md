# GLib 0.18 VariantStrIter security backport

This public compatibility branch preserves the GLib 0.18 API required by GTK3 consumers. It applies the official upstream fix for RUSTSEC-2024-0429 / GHSA-wrw7-89jp-8q8g: make the C out-pointer mutable and pass `&mut p` to `g_variant_get_child`.

- Upstream base: gtk-rs/gtk-rs-core branch `0.18`, commit `42b9caf98e03ded086362d9653ca58fe94dc8658`.
- Original fix: https://github.com/gtk-rs/gtk-rs-core/pull/1343, merge commit `05dff0ee696f9bcd8617cd48c4b812d046d440cb`.
- The fix is cherry-picked with `-x`, retaining original authorship and commit provenance.
- Package licenses remain MIT; upstream license files and notices are preserved.
- Package versions are intentionally unchanged. This is a source backport, not a claim that an upstream 0.18 registry release is fixed. Version-only advisory scanners may still report the published vulnerable range; consumers must record the exact fixed source revision and evidence when triaging that finding.

Consumers should pin this source by commit and unify GLib, its macros, GLib/GObject/GIO sys crates to avoid mixing incompatible raw FFI types. Validate string-array iteration (`next`, `next_back`, `nth`, `nth_back`, `last`, empty and Unicode values) in an optimized build, because debug-only tests can miss the original undefined behavior. Native GUI and database-adapter acceptance remain each consumer's responsibility.

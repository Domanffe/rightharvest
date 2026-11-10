# Publishing

The artifact is built and ready. Only the submit form is left, and it needs a GitHub sign-in, which
only you can do.

## The file

`C:/Users/world/Documents/HermesWorkspace/mods/rightharvest/dist/rightharvest-0.2.0.hnmod` — 3769 bytes
SHA-256 `0e819796fdbcdd920e81587e59f55f2bc94777f421b0af54504851bfeda52efa`

It holds `horizon-native.json` (game 26.3, schema 1, hooks 21, pinned to Horizon `fdc87ae` and
horizon-mixins `e8cd0aa`) and `mixins/rightharvest/`.

## The form

https://playhorizon.dev/mods/submit — GitHub sign-in, then:

- **Kind**: Native mod (the dropdown does not default to it)
- **Project id**: `rightharvest` (free text; the existing id is what makes this an update)
- **Version**: `0.2.0`
- **For Minecraft**: `26.3`
- **Name**: RightClickHarvest
- **Summary** (≤160 chars): Right-click a ripe crop to pick it and sow it again.
- **Description** (first paragraph only is kept): Empty hand, right click a ripe wheat, carrot, potato,
  beetroot or nether wart. You get the usual drops, and the crop goes right back into the soil if you
  carry a seed. Growing crops, other blocks and a hand holding an item are left alone.
- **Authors**: Alexander (Domanffe)
- **Licence**: MIT
- **Source**: https://github.com/Domanffe/rightharvest
- **File**: `rightharvest-0.2.0.hnmod`
- **Changelog**: Rebuilt for the 8 November nightly. Gameplay is unchanged: one right click still
  harvests a ripe crop and replants it. The mod now hangs on the game's own right-click event instead
  of a private function, so future Horizon updates won't quietly turn it off.
- Public key and signature: leave empty (unsigned).

## What was checked

- `horizon-mixins apply` of the packaged bytes against the `fdc87ae` tree: `status: ok`, 2 injections,
  1 module.
- `cargo check --locked --profile release -p horizon-client` on that staged tree: finished, no errors.
- `cargo test` in the mod's own crate: 5 passed; `cargo fmt` and `cargo clippy` clean.
- Not run: the build in the launcher and a harvest in the running game — `PROOF.md` lists the steps.

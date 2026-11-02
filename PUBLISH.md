# Publishing

The artifact is built and ready. Only the submit form is left, and it needs a GitHub sign-in, which
only you can do.

## The file

`C:\Users\world\Documents\HermesWorkspace\mods\rightharvest\dist\rightharvest-0.1.1.hnmod` — 3856 bytes
SHA-256 `2f70f3a5abaca35f1ff8df7b198f0fd616eacaa5229f5113f75935e16f50255b`

It holds `horizon-native.json` (game 26.3, schema 1, hooks 2, pinned to Horizon `4aa6854` and
horizon-mixins `a2af5c0`) and `mixins/rightharvest/`.

## The form

https://playhorizon.dev/mods/submit — GitHub sign-in, then:

- **Project id**: `rightharvest` (free text; the existing id is what makes this an update)
- **Name**: RightClickHarvest
- **Summary** (≤160 chars): Right-click a ripe crop to pick it and sow it again.
- **Description** (first paragraph only is kept): with an empty hand, right-click a ripe wheat, carrot,
  potato, beetroot or nether wart: the crop is harvested with the game's own drops, sound and
  statistics, and sown again from a seed in your inventory.
- **Authors**: Alexander (Domanffe)
- **Licence**: MIT
- **Source**: https://github.com/Domanffe/rightharvest
- **File**: `rightharvest-0.1.1.hnmod`
- **Changelog**: Rebuilt on the 2026-11-01 Nightly (Horizon `4aa6854`); the mods hook is named instead
  of pinned to a source target, so it survives a Horizon update that moves that point.
- Public key and signature: leave empty (unsigned).

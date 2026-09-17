# Frozen bundle artifacts

The `.plugin` bundle files and `_extracted/` mirror directories in this
directory (`loreconvo-v0.10.10.plugin`, `loreconvo_extracted/`,
`loredocs-v0.1.27.plugin`, `loredocs_extracted/`) are **frozen historical
snapshots**, effective 2026-09-17 (SH-101285, approved architecture proposal
`docs/agent-reports/architecture/proposals/marketplace_plugin_bundle_retirement_20260827.md`).

- **No release rebuilds these artifacts anymore.** `scripts/update_marketplace_json.sh`
  (formerly `rebuild_marketplace_bundles.sh`) only refreshes
  `marketplace/claude-plugins/.claude-plugin/marketplace.json`'s `version`
  and `source.ref` fields.
- **No install path reads these artifacts.** `marketplace.json` declares
  both plugins with `source: {source: github, repo: labyrinth-analytics/<product>,
  ref: vX.Y.Z}` -- `/plugin install` resolves through that github source, not
  through any file in this directory. Independently re-verified 2026-08-27
  against both the documented Cowork install flow and the public per-product
  repos.
- **Known defect, left uncorrected on purpose:** `loreconvo-v0.10.10.plugin`'s
  (and every version since `v0.10.6`'s) `plugin.json` declares SessionStart /
  Stop / SessionEnd hooks that the packaged zip does not contain (the bundle
  build never packed `hooks/`, since its creation in commit `cdca99f37`,
  2026-05-12). This will NOT be corrected in place -- doing so would mutate an
  already-published, version-pinned release artifact, which breaks
  immutability expectations for anyone who has cached, mirrored, or pinned a
  copy of that exact filename. If these bundles are ever rebuilt for a real
  consumer, the hooks declaration must be corrected as part of that rebuild,
  not before.

If a zip-sourced install path is designed in the future, that design starts
from a fresh rebuild (correcting the hooks mismatch as part of that work),
not from patching these frozen files.

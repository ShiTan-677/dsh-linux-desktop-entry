# Vendored shell kit

`vendor/linux-dsh-web-entry/` is a verbatim copy of the tracked tree of the
sibling repository **`linux-dsh-web-entry`**, taken at:

| Field | Value |
| --- | --- |
| Commit | `d2c81c16adb293823c4fb0eb837a91cfe227bf8e` |
| `VERSION` | `0.1.0` |
| Vendored on | 2026-10-14 |

## Why a copy instead of a dependency

The two repositories are deliberately independent. Each one stands on its own:
neither breaks when the other is unpublished, unavailable, or moved. The plugin
therefore carries the shell kit it drives rather than resolving it at install
time.

The price of independence is drift, so the copy is made *detectable* rather than
merely documented:

- `tools/selftest-plugin.mjs` compares the vendored tree against the sibling
  checkout at `../linux-dsh-web-entry` and fails on any difference. When the
  sibling is absent — a normal clone of this repository on its own — it skips
  that comparison and says so.
- Re-vendoring is a deliberate act, not an automatic one: copy the tracked tree,
  then update the commit above.
  `git -C ../linux-dsh-web-entry ls-files` lists exactly what to copy.
- That a file differs from upstream is not by itself a defect. A deliberate local
  change must be recorded here, with its reason, so the next sync does not
  silently revert it.

## What the plugin relies on

The Host half runs `vendor/linux-dsh-web-entry/install.sh` and
`vendor/linux-dsh-web-entry/bin/dsh-web-open.sh`. The desktop entry, the icon
composition, the launcher, the optional `systemd --user` service, and the
`--uninstall` path all come from this one copy. No install, remove or launch
logic is reimplemented in JavaScript here.

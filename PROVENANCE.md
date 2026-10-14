# Vendored shell kit

`vendor/linux-dsh-web-entry/` is a verbatim copy of the **runtime payload** of the sibling
repository `linux-dsh-web-entry`, taken at:

| Field | Value |
| --- | --- |
| Commit | `d2c81c16adb293823c4fb0eb837a91cfe227bf8e` |
| `VERSION` | `0.1.0` |
| Vendored on | 2026-10-14 |

The sibling's documentation is deliberately not copied; see [The payload](#the-payload).

## Why a copy instead of a dependency

The two repositories are deliberately independent. Each one stands on its own: neither breaks
when the other is unpublished, unavailable, or moved. The plugin therefore carries the shell kit
it drives rather than resolving it at install time.

The price of independence is drift, so the copy is made *detectable* rather than merely
documented:

- `tools/selftest-plugin.mjs` checks the payload list below against the sibling checkout at
  `../linux-dsh-web-entry`: every listed file must exist upstream and match byte-for-byte, and
  the vendored tree must contain nothing outside the list. Listing the payload is what catches
  an upstream *addition* that the copy never picked up. When the sibling is absent — a normal
  clone of this repository on its own — the comparison is skipped and says so.
- Re-vendoring is a deliberate act, not an automatic one: copy the payload files, then update
  the commit above.
- A file differing from upstream is not by itself a defect. A deliberate local change must be
  recorded here, with its reason, so the next sync does not silently revert it.

## The payload

| File | Why the plugin carries it |
| --- | --- |
| `install.sh` | the only implementation of install, uninstall, and `--systemd` |
| `bin/dsh-web-open.sh` | the launcher the desktop entry runs |
| `share/dsh-web.desktop.in` | desktop entry template rendered by the installer |
| `share/dsh-web.service.in` | systemd unit template rendered by the installer |
| `tools/compose-icon.py` | composes the icon from the installed DSH frontend favicon |
| `tools/selftest.sh` | tests this vendored copy in place |
| `LICENSE` | redistribution notice, required in order to carry the copy at all |
| `VERSION` | records which upstream release the payload came from |
| `.gitignore` | keeps Python bytecode out of this repository |

The shell project's `README.md`, `README.zh-CN.md`, `CHANGELOG.md`, and `docs/` are **not**
vendored. They are not part of what runs, and comparing prose byte-for-byte would turn every
upstream documentation edit into a failed check — noise, not the drift that matters.

## What the plugin relies on

The Host half runs `vendor/linux-dsh-web-entry/install.sh` and
`vendor/linux-dsh-web-entry/bin/dsh-web-open.sh`. The desktop entry, the icon composition, the
launcher, the optional `systemd --user` service, and the `--uninstall` path all come from this
one copy. No install, remove, or launch logic is reimplemented in JavaScript here.

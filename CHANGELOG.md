# Changelog

## 0.1.2 — 2026-11-14 (patch)

- Re-vendor `linux-dsh-web-entry` 0.1.2: `install.sh --uninstall` no longer reaches the
  live user manager from a custom unit directory. The plugin self-test uninstalls from a
  temporary tree, so on a machine with the unit enabled every self-test run used to
  disable the real `dsh-web.service`; it no longer does.
- The vendored `tools/selftest.sh` is now 43 checks (was 41).

## 0.1.1 — 2026-11-14 (patch)

- Re-vendor `linux-dsh-web-entry` 0.1.1: the launcher no longer hands its start-lock
  descriptor to the browser or the terminal it spawns, and truncates the shared launch log
  only after taking the lock. On the real desktop the leaked lock showed up as a leftover
  `chrome` holding `~/.local/state/dsh-web/start.lock`, which made every later launch time
  out waiting for a token.
- The vendored `tools/selftest.sh` is now 41 checks (was 36).

## 0.1.0 — 2026-10-14 (public preview)

- Add a DSH bundle that manages the [Linux DSH Web Entry](https://github.com/ShiTan-677/linux-dsh-web-entry)
  desktop entrance from inside the harness: a Settings page, a `desktop_entry` agent tool, and a
  `/desktop-entry` command, all three driving one implementation.
- Carry a verbatim copy of the shell project's runtime payload under `vendor/`, so the two
  repositories stay independent: the plugin runs the same `install.sh` rather than reimplementing
  install, remove, or launch. `PROVENANCE.md` pins the revision, and the plugin self-test compares
  the copy against the sibling checkout whenever one exists.
- Expose the resident `systemd --user` service as opt-in on every surface, off by default, with the
  resource cost stated in the UI.
- Report that service in the launcher's own terms: the path the launcher reads as well as the path
  the installer writes, plus `is-enabled` and `is-active`. A unit file the launcher never reads is
  reported as what it is, not as a resident service.
- Write nothing to the desktop at install time: the page shows the state on disk and waits for a
  click.

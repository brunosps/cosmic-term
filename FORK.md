# Manual tab renaming

This fork of [pop-os/cosmic-term](https://github.com/pop-os/cosmic-term) adds a
manual title for each terminal tab. It is based on version 1.11.0, commit
`8a05e91948814afc4162d9fcdc95875c8cc69400`.

## Usage

- Press **Ctrl+Shift+R**, choose **File → Rename tab**, or use the terminal's
  context menu.
- Enter confirms the name; Escape cancels.
- An empty name restores the current automatic title immediately.
- Program and shell title updates leave a manual name in place.
- Editing shortcuts act on the dialog; tab-changing shortcuts are blocked
  while a name is being edited.
- The dialog and menu labels include Brazilian Portuguese translations.

## Attribution and local changes

The initial implementation is by **vacenty**, from
[upstream PR #931](https://github.com/pop-os/cosmic-term/pull/931), original commit
`eee6409fd7fcc1660608f4ee97a2b6b7afad02c7`. It is recorded as a separate backport
commit with its original author.

The following commit adds menu entries, the pt-BR translation, the action in the
shortcut settings list, keyboard and focus isolation for the dialog, and a cached
automatic title so clearing a manual name takes effect immediately. It also
allows a terminal process exiting during a rename to close its tab normally.

The upstream GPL-3.0-only license is retained. This is an independent fork;
upstream has not merged this feature as of November 9, 2026.

## Build

Use the system development dependencies listed in
[the Rust checks workflow](.github/workflows/rust-checks.yml).

```sh
git clone https://github.com/brunosps/cosmic-term.git
cd cosmic-term
cargo build --release --locked -j 3
./target/release/cosmic-term --no-daemon
```

## Validation

Validated locally on COSMIC/Wayland on November 9, 2026:

- Release build and all **12 existing Rust tests** passed.
- `cargo fmt --check` and `git diff --check` passed.
- `cargo clippy --release --all-features --locked -- -W clippy::pedantic`
  completed without errors. Existing warnings remain; none pointed at added
  lines after the local fixes.
- A real UI test used a virtual keyboard and two isolated PTY peers that only
  recorded input. It checked Unicode names, per-tab isolation, persistence over
  OSC title updates, empty-name reset, Escape cancellation, and blocking tab
  creation while editing. No dialog input reached either PTY.
- Super+T opened the locally installed binary, and Ctrl+Shift+R renamed the new
  terminal. Existing terminal processes remained running.

These are local validation results; no GitHub Actions result is implied.

# Forge – ikaru5 fork

Forge is a GNOME Shell extension that provides tiling/window management. Upstream
([forge-ext/forge](https://github.com/forge-ext/forge)) is
[looking for a maintainer](https://github.com/orgs/forge-ext/discussions/276).

## Why this fork

This is a personal fork. I ([@ikaru5](https://github.com/ikaru5)) have used Forge
intensively for years and depend on it every day, on a laptop and on a multi-monitor
desktop. I run Arch, so I am usually on the newest GNOME within days of its release,
which is exactly when Forge tends to break: GNOME 51 came out in September 2026, and
in October it still crashed the shell when opening Chrome. Waiting for an
extensions.gnome.org release is not an option for me, so this fork has its own
release channel.

What you get:

- **Close to upstream.** Patches are small, targeted fixes for current GNOME, written
  to be upstreamable. No refactoring for its own sake. If a fix is not fork-specific
  it is also sent upstream.
- **Every commit is reviewed by a human.** I am a senior software developer (games and
  web) with no GNOME Shell background. The code is written with Claude (Anthropic;
  Opus and Fable), and I review, test and merge every commit myself. Nothing lands
  here that I have not read and understood. That is also the security argument: at
  least I know what is in it, and the diff to upstream stays small enough for anyone
  else to check.
- **Testing is manual**, on the setups above, through daily use. There is no test
  suite for a tiling window manager that would be worth more than that.
- **Releases are built by GitHub Actions** from tags, and a small updater script keeps
  all my machines current without going through the extensions.gnome.org review. See
  [Installation](#installation).

### Relation to jcrussell/forge

[jcrussell/forge](https://github.com/jcrussell/forge) is an AI-driven fork with a much
larger scope: more than 400 commits of refactoring and new features, currently under
review for extensions.gnome.org. I cannot audit a change set of that size, so I cannot
vouch for it, and as of October 2026 it does not support GNOME 51 yet.

That said, it is the more ambitious project. Once it is established on
extensions.gnome.org and keeps up with GNOME releases, I will likely switch to it and
retire this fork.

## Installation

Requires `curl`, `jq` and `gnome-extensions` (part of GNOME Shell).

```bash
mkdir -p ~/.local/bin ~/.config/autostart
curl -fLo ~/.local/bin/forge-updater https://raw.githubusercontent.com/ikaru5/forge/main/updater/forge-updater
curl -fLo ~/.config/autostart/forge-updater.desktop https://raw.githubusercontent.com/ikaru5/forge/main/updater/forge-updater.desktop
chmod +x ~/.local/bin/forge-updater && forge-updater
```

Then log out and back in, and enable the extension once:

```bash
gnome-extensions enable forge@jmmaranan.com
```

Both files are plain text, read them before running: [forge-updater](updater/forge-updater),
[forge-updater.desktop](updater/forge-updater.desktop).

### Updates

`forge-updater` runs at every login. It asks GitHub for the latest release, and if it
is newer than the installed version it downloads the zip and installs it with
`gnome-extensions install`. Nothing from the download is executed. A notification
tells you the new version is active after the next login. Run `forge-updater` by hand
to update right away; its output is in `journalctl --user -g Forge`.

The fork's version numbers are higher than the extensions.gnome.org ones, so the GNOME
Extensions app will not replace it with the upstream release. Disable automatic
extension updates in the Extensions app to be sure.

To stop updating, delete the two files:

```bash
rm ~/.local/bin/forge-updater ~/.config/autostart/forge-updater.desktop
```

### Releases

A tag `vX.Y.Z` on `main` triggers [fork-release.yml](.github/workflows/fork-release.yml),
which builds the extension, writes the version into `metadata.json`
(`version = X*10000 + Y*100 + Z`, `version-name = X.Y.Z`) and publishes a GitHub release
with the zip and a `SHA256SUMS` file.

```bash
git tag v1.0.1 && git push origin v1.0.1
```

## Features

- Works on GNOME 45 through 51. X11 and Wayland
- Tree-based tiling with vertical and horizontal split containers similar to i3-wm and sway-wm
- Vim-like keybindings for navigation/swapping windows/moving windows in the containers
- Drag and drop tiling
- Support for floating windows, smart gaps and focus hint
- Customizable shortcuts in extension preferences
- Some support for multi-display
- Tiling support per workspace
- Update hint color scheme from preferences
- Stacked tiling layout
- Swap current window with the last active window
- Auto Split or Quarter Tiling
- Show/hide tab decoration via keybinding https://github.com/forge-ext/forge/issues/180
- Window resize using keyboard shortcuts

## Known Issues / Limitations

- Does not support dynamic workspaces
- Does not support vertical monitor setup

![image](https://user-images.githubusercontent.com/348125/146386593-8f53ea8b-2cf3-4d44-a613-bbcaf89f9d4a.png)

## Forge Keybinding Defaults

See the acceptable key combinations on the [wiki](https://github.com/forge-ext/forge/wiki/Keyboard-Shortcuts)

| Action | Shortcut |
| --- | --- |
| Increase active window size left | `<Ctrl> + <Super> + y` |
| Decrease active window size left | `<Ctrl> + <Shift> + <Super> + o` |
| Increase active window size bottom | `<Ctrl> + <Super> + u` |
| Decrease active window size bottom | `<Ctrl> + <Shift> + <Super> + i` |
| Increase active window size top | `<Ctrl> + <Super> + i` |
| Decrease active window size top | `<Ctrl> + <Shift> + <Super> + u` |
| Increase active window size right | `<Ctrl> + <Super> + o` |
| Decrease active window size right | `<Ctrl> + <Shift> + <Super> + y` |
| Open preferences | `<Super> + period` |
| Toggle tiling mode |`<Super> + w` |
| Focus left | `<Super> + h` |
| Focus right | `<Super> + l` |
| Focus up | `<Super> + k` |
| Focus down | `<Super> + j` |
| Swap current window with last active | `<Super> + Return` |
| Swap active window left | `<Ctrl> + <Super> + h` |
| Swap active window right | `<Ctrl> + <Super> + l` |
| Swap active window up | `<Ctrl> + <Super> + k` |
| Swap active window down | `<Ctrl> + <Super> + j` |
| Move active window left | `<Shift> + <Super> + h` |
| Move active window right | `<Shift> + <Super> + l` |
| Move active window up | `<Shift> + <Super> + k` |
| Move active window down | `<Shift> + <Super> + j` |
| Split container horizontally | `<Super> + z` |
| Split container vertically | `<Super> + v` |
| Toggle split container | `<Super> + g` |
| Gap increase | `<Ctrl> + <Super> + Plus` |
| Gap decrease | `<Ctrl> + <Super> + Minus` |
| Toggle focus hint | `<Super> + x` |
| Toggle active workspace tiling | `<Shift> + <Super> + w` |
| Toggle stacked layout | `<Shift> + <Super> + s` |
| Toggle tabbed layout | `<Shift> + <Super> + t` |
| Show/hide tab decoration | `<Ctrl> + <Alt> + y` |
| Activate tile drag-drop | `Start dragging - Mod key configuration in prefs` |
| Snap active window left two thirds | `<Ctrl> + <Alt> + e` |
| Snap active window right two thirds | `<Ctrl> + <Alt> + t` |
| Snap active window left third | `<Ctrl> + <Alt> + d` |
| Snap active window right third | `<Ctrl> + <Alt> + g` |
| Persist toggle floating for active window | `<Super> + c` |
| Persist toggle floating for active window and its window class | `<Super><Shift> + c` |

For any shortcut conflicts, the user has to manually configure those for now from the
`GNOME Control Center > Keyboard > Customize Shortcuts`. https://github.com/forge-ext/forge/issues/37

## Forge Override Paths

- Window Overrides: `$HOME/.config/forge/config/windows.json`
- Stylesheet Overrides: `$HOME/.config/forge/stylesheet/forge/stylesheet.css`

## GNOME Defaults

GNOME Shell has built in support for workspace management and seems to work well - so Forge will not touch those.

User is encouraged to bind the following:
- Switching/moving windows to different workspaces
- Switching to numbered, previous or next workspace

## Development

- Install NodeJS 16+, `gettext` and `glib2` (for `glib-compile-schemas`)
- Run `npm install`
- Commands:

```bash
# Build and install into ~/.local/share/gnome-shell/extensions, then log out and back in
make build && make install

# Same, with debug logging enabled
make build && make debug && make install

# X11 only - build from source and restart gnome-shell
make test-x

# Formatting. husky runs prettier on commit after npm install
npm run format
```

Every build rewrites `po/*.po` via `msgmerge`; discard those changes unless you are
updating translations.

## Contributing

- Please be nice, friendly and welcoming on discussions/tickets.
- Bugs in this fork: [ikaru5/forge/issues](https://github.com/ikaru5/forge/issues).
  Fixes that are not fork-specific are also sent upstream.

## Credits

Thank you to:
- Jose Maranan and all Forge contributors
- Michael Stapelberg/contributors for i3
- System76/contributors for pop-shell
- ReworkCSS/contributors for css-parse/css-stringify

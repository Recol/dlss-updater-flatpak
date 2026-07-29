# DLSS Updater — Flatpak repository

This repository hosts the [OSTree](https://ostreedev.github.io/ostree/) repository that
gives the Linux builds of [DLSS Updater](https://github.com/Recol/DLSS-Updater) an update
channel.

It holds no source code. The application lives in
[Recol/DLSS-Updater](https://github.com/Recol/DLSS-Updater).

## Why this exists

The `.flatpak` bundles attached to DLSS Updater's GitHub releases used to be built without
an origin remote, which meant `flatpak update` could never find a newer version — the only
way to move to a new release was to download the next bundle by hand.

Bundles are now built with `--repo-url` pointing at this repository, so installing one
automatically configures a remote. `flatpak update` works from then on, and desktop
software centres (GNOME Software, KDE Discover) pick up new releases on their own.

## Adding the remote manually

Installing a recent `.flatpak` bundle sets this up for you. To add it by hand:

```bash
flatpak remote-add --user --if-not-exists \
    dlss-updater https://recol.github.io/dlss-updater-flatpak/dlss-updater.flatpakrepo
flatpak install --user dlss-updater io.github.recol.dlss-updater
```

Then, to update:

```bash
flatpak update --user io.github.recol.dlss-updater
```

## Layout

| Branch | Contents |
| --- | --- |
| `main` | This README and the repository metadata template. Normal history. |
| `gh-pages` | The published OSTree repository, served at https://recol.github.io/dlss-updater-flatpak/. Replaced wholesale on every publish so the git history stays a single commit and the repository does not accumulate every object ever built. |

## How it is published

`publish_flatpak_repo.sh` in the main DLSS-Updater repository takes the local `repo/`
directory produced by `build_flatpak.sh`, runs `flatpak build-update-repo` over it to
regenerate the summary, generate static deltas and prune old history, then force-pushes
the result to `gh-pages`.

## Relationship to Flathub

Independent of it. When DLSS Updater is published on Flathub it will use a separate
application ID (`io.github.recol.dlss_updater`) and Flathub's own infrastructure; that
build never consults this repository. This channel serves the `.flatpak` bundles attached
to GitHub releases (`io.github.recol.dlss-updater`).

## Licence

DLSS Updater is licensed under the AGPL-3.0. See the
[main repository](https://github.com/Recol/DLSS-Updater/blob/main/LICENSE).

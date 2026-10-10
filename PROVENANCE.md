# Provenance

Which parts of this repository came from where.

## Qinux

Everything in this repository is original work by the Qinux project, written
against the Qinux specification and the Qinux palette.

## Nexus Linux

The following files were informed by
[Nexus-Linux-Official/NexusLinux](https://github.com/Nexus-Linux-Official/NexusLinux),
an earlier Arch-based distribution by the same author. Both are GPL-3.0
covering: Nexus's root licence is GPL-2.0 and its package declarations are
GPL-3.0-or-later, so nothing from it was copied verbatim into a file that
ships as GPL-3.0-only.

What was taken was the **approach**, not the text:

| Qinux file | Inspired by | What was kept |
| --- | --- | --- |
| `config/kwinrc` | Nexus `kwinrc` | The structure: compositing block, per-effect enabled flags, a plugin list, window placement |
| `config/kdeglobals` | Nexus `kdeglobals` | The set of keys a Qinux session actually needs to pin |
| `config/plasmarc` | Nexus `plasmarc` | Per-theme `[Theme-*]` blocks so each toolkit is set explicitly |
| `plasma/config/default-org.kde.plasma.desktop-appletsrc` | Nexus `plasma-org.kde.plasma.desktop-appletsrc` | Containment and applet numbering, `knownItems` for tray order |
| `initramfs/mkinitcpio.conf.d/qinux.conf` | Nexus `archiso.conf` | Hook set for a live image, and the reasoning for each |
| `hardware/modprobe.d/qinux-nvidia.conf` | Nexus `nvidia-loader.conf` | The indirection: module name to loader, so driver generation is chosen by PCI ID |
| `boot/grub/theme.txt` | Nexus grub `theme.txt` | The layout percentages and the colour-field names GRUB expects |
| `wallpapers/source/generate.py` | Nexus `nexus-wallpapers` PKGBUILD | Generating wallpapers from a recipe rather than shipping bitmaps |

Everything above was rewritten: Qinux paths, Qinux palette, Qinux rationale in
the comments, and several decisions reversed. See the notes below.

## Upstream

| What | From |
| --- | --- |
| `kde/config/kdeglobals` defaults | Breeze |
| `kde/config/kwinrc` defaults | KWin |
| `kde/config/plasmarc` defaults | Plasma |
| sysctl defaults in `system/sysctl.d/90-qinux.conf` | Linux kernel documentation |
| Package and repository formats | pacman |

Qinux deliberately inherits these rather than replacing them. A desktop that
forks its toolkit's defaults ends up carrying fixes that upstream already made.

## Decisions that differ from Nexus

Recorded because they look like omissions and are not.

| Decision | Nexus | Qinux | Why |
| --- | --- | --- | --- |
| Compositor effects | Present Windows, Desktop Grid, Window Aperture on | Blur, Slide, SlideBack only | The others fight with a tiling-adjacent layout and a borderless maximised window |
| Animation | Enabled | Disabled | The project brief is an aesthetic one; this is the setting that expresses it |
| Wallpapers | PIL, three themes | Hand-written PNG encoder, two themes | Drops the Pillow build dependency, and the output is byte-identical across runs |
| Live session logo | Binary PNG per size | Generated from `generate.py` | The recipe ships, so a colour change is a rebuild rather than a redraw |
| Compression | `xz` | `zstd -19 --long` | Faster on the machines Qinux targets, smaller than xz at the same effort |
| Profile images | Not separated | `desktop-kde` and `installer` inherit `base` | One definition of the base set, so a change lands in every profile at once |

## Adding to this file

When a file is derived from somewhere else, add a row. A reviewer who cannot
tell where a default came from will assume it was invented, and a default with
no provenance cannot be compared against the thing it was copied from when that
thing changes.
# CLAUDE.md

This repo contains a [Rockbox](https://www.rockbox.org/) theme based on SNARTY (v1.0, by Simon Andén / Tirim4, CC-BY-SA 4.0). SNARTY itself derives from SPAZZ, SNAZZ/SNAZZ2 and SNAZZY, with some elements from adwaitapod. Keep the attribution headers in the theme files and keep derived work under CC-BY-SA 4.0.

The goal of this fork is a variant of SNARTY without album art. Upstream draws album art in the `.wps` in the `#__Album Art` blocks of the main display and the lock screen (`%Cl`/`%Cd`, with `wps/SNARTY/AlbumArt.bmp` as the placeholder).

- Target resolution: 320x240 (tested upstream on the HiFi Walker H2)
- Files are still named `SNARTY.*`; the theme has not been renamed in this fork

## Layout

The tree mirrors the `.rockbox/` directory on the player.

| Path | Purpose |
| --- | --- |
| `themes/SNARTY.cfg` | Theme config: points at the wps/sbs/iconset/font and sets colours and UI settings |
| `wps/SNARTY.wps` | While Playing Screen skin (main display plus lock screen shown when hold is on) |
| `wps/SNARTY.sbs` | Base skin for menus: status bar, title, now-playing footer, quick screen, USB screen |
| `wps/SNARTY/*.bmp` | Images loaded by the skins with `%xl` |
| `icons/*.bmp` | Menu and viewer iconsets |
| `fonts/*.fnt` | KodeMono in 12/15/22/29/72 px, Rockbox `.fnt` format |

## Installing and testing

There is no build step, linter or test suite. To try a change, copy `fonts/`, `icons/`, `themes/` and `wps/` into `/.rockbox/` on the player (or a Rockbox simulator's `simdisk/.rockbox/`) and select the theme under Settings > Theme Settings > Browse Theme Files.

Paths in `SNARTY.cfg` are absolute device paths (`/.rockbox/...`), so renaming or moving a file means updating the `.cfg`, and the `%xl`/`%Fl` references in both skin files.

## Skin conventions

- `.wps` and `.sbs` use Rockbox skin tags; reference: https://www.rockbox.org/wiki/CustomWPS
- Lines starting with `#` are comments. Section labels use `#__Name` and `#--- Name ---#`. A line without the leading `#` is rendered as text on screen.
- Fonts are preloaded with `%Fl(id,file)` into slots 2-6 (12, 15, 22, 29, 72 px) and referenced by slot number as the last argument of a viewport. The `.sbs` does not load slot 2.
- Viewports are `%V(x,y,w,h,font)`; named ones are `%Vl(name,...)` shown with `%Vd(name)`. Negative x/y is measured from the right/bottom edge. Layout is hand-positioned in pixels for 320x240, with a 20 px side margin.
- `%Vi(name,...)` / `%VI(name)` define and select the UI viewport that Rockbox draws menus into.
- Images are 1-bit BMPs, drawn in the foreground/background colours from the `.cfg`. Multi-state images are vertical strips: the subimage count in `%xl(id,file,x,y,count)` must match the BMP (strip height = count x frame height).
- Colours are set only in the `.cfg` (`background color: 232234`, `foreground color: a2accd`); highlights are done with `%Vs(invert)`.
- The status bar, hold indicator, shuffle/repeat and battery blocks are duplicated between the `.sbs` and the lock screen section of the `.wps`; change both together.

<div align="center" width="100%">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./img/icon-light.svg" width="128">
    <source media="(prefers-color-scheme: light)" srcset="./img/icon-dark.svg" width="128">
    <img alt="" src="./img/icon-light.svg" width="128" />
  </picture>
</div>

# Better speed adjustments for Klipper

Individually adjust speeds for walls, infill, support and more!

Currently works with Cura, please add support for other slicers!

Supported line types:

- infill (TYPE_FILL)
- skin (TYPE_SKIN) (first/last few layers)
- skirt (TYPE_SKIRT) (also brim/raft)
- support (TYPE_SUPPORT)
- wall_inner (TYPE_WALL_INNER)
- wall_outer (TYPE_WALL_OUTER)

If you find more, please add them or open an issue! \
Any unknown line types should only result in a warning `Unknown command`.

## How it works

This script utilizes the `;TYPE:` comment by Cura.
The comments are replaced with `TYPE_` to make them a macro call.

Then, a macro `SET_SPEEDS` is called by the user to set the speed for each line type.
While printing, the `TYPE_` macros set `M220` to the corresponding speed.

**IMPORTANT: once you set one of the speeds, DO NOT USE M220,
neither the speed slider! This macro will overwrite it.**

## Usage

### UI

The macro UI of your klipper/moonraker frontend is probably the most intuitive option. \
**Speeds are in percent!**

> [!TIP]
> hiding the `TYPE_` macros will prevent clutter.

### in terminal

```
; to get current speeds
SET_SPEEDS

; to set speeds, e.g. 100% for everything
SET_SPEEDS INFILL=100 SKIN=100 SKIRT=100 SUPPORT=100 WALL_INNER=100 WALL_OUTER=100

; to disable custom speed control and use M220 / speed slider again
DISABLE_CUSTOM_SPEEDS
```

## Installation

### Install gcode macros

Include [`betterspeedadjust.cfg`](./betterspeedadjust.cfg) in your printer config.
There are multiple ways, I have the file in `macros/`
and include it with `[include macros/*.cfg]`.

If you don’t already know what to do, download [`betterspeedadjust.cfg`](./betterspeedadjust.cfg),
upload it to your printer’s config directory and include it by
putting `[include betterspeedadjust.cfg]` into your `printer.cfg`.

### Configure Cura

In Cura, go to `Extensions` > `Post Processing` > `Modify G-Code`
and add a new script `Search and Replace`:

Search: `;TYPE:` \
Replace: `TYPE_`

Add two more ***after the one above (IMPORTANT)***:

Search: `TYPE_WALL-INNER` \
Replace: `TYPE_WALL_INNER`

and

Search: `TYPE_WALL-OUTER` \
Replace: `TYPE_WALL_OUTER`

Leave regex unchecked for all three.

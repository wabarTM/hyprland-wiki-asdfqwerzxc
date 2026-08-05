---
title: Naming conventions
weight: 20
---

## Syntax <!-- TODO: make a better name for that section -->

> [!INFO]
> For the ease of understanding wiki introduces a special syntax for some elements.

### Optional parameters

Text followed by a question mark (?) indicates that the parameter is optional and may be omitted.
If the question mark is followed by `= data_type`, this means that the parameter can only take the specified data_type or defaults to a specified value.

For example: `hl.dsp.window.float({ window?, action? })` means that `window` and `action` are not required. The dispatcher will use their default values instead: `activewindow` for window and `toggle` for action.

### Value ranges

Options: [value1 - value2] means a range of values from value1 to value2 with respect to the type. For example: `Options: [0.25 - 5.0]` means all floating numbers from 0.25 to 5.0 are allowed

### Coordinates

Coordinates are in inverse Y cartesian system, so from top-left corner of the monitor to the right is positive x (+x) and down is positive y (+y).

## Data types

| type | description |
| --- | --- |
| int | Integer number |
| float | Floating point number |
| bool | Boolean, `true` or `false` |
| str | Lua string. Symbols wrapped in `""`/`[[]]`/`''`, e.g: `"dwindle"`/`[[master]]`/`'scrolling'`. When using lua multiline string (`[[]]`) escaping of `"` and `'` is not needed |
| vec2 | Vector with 2 float values. { x, y }, e.g. `{ 20, 20 }` |
| css_gaps | An integer, or `{ top?, left?, right?, bottom? }` |
| color | Color. See hint below for color info |
| gradient | A gradient, will accept a color, or `{ colors = { color, color }, angle? = float }` structure |
| font_weight | An integer between 100 and 1000, or one of the following presets: `thin` (100) `ultralight` (200) `light` (300) `semilight` (350) `book` (380) `normal` (400) `medium` (500) `semibold` (600) `bold` (700) `ultrabold` (800) `heavy` (900) `ultraheavy` (1000) |

There are implicit conversions between certain types, however, this may lead to undefined behaviour later. Lsp with lua stub can be used to warn about the use of wrong type. More on that can be read [here](../configuring/core#autocompletions)

### Colors

You have 4 options:
- web-styled hash in RGBA: `"#fafc21"` or `"#ddd"` or `"#fa3d7bff"`
- rgba(): `"rgba(b3ff1aee)"`, or decimal equivalent `"rgba(179,255,26,0.933)"`
- rgb(): `"rgb(b3ff1a)"`, or the decimal equivalent  `"rgb(179,255,26)"`  
Note: Decimal rgba/rgb values should have no spaces between numbers.
- legacy in ARGB: `0xeeb3ff1a`

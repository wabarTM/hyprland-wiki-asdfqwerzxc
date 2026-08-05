---
weight: 50
title: Binds
---

## Basics

```lua
hl.bind("keys", dispatcher or function() , {bind_flags})
```

for example,

```lua
hl.bind("SUPER + SHIFT + Q", hl.dsp.exec_cmd("firefox"))
```

will bind opening Firefox to <key>SUPER</key> + <key>SHIFT</key> + <key>Q</key>

will bind opening Firefox to SUPER + SHIFT + Q

<!-- TODO: why on earth key is needed there? wiki parsing? -->

The dispatcher list can be found in
[Dispatchers](../dispatchers/#list-of-dispatchers).

You can also put a lua function if you prefer as your bind dispatcher:

```lua
hl.bind("SUPER + SHIFT + X", function()
    -- some logic...
    hl.dispatch(hl.dsp.window.float({ action = "toggle" }))
end)
```

## Uncommon syms / binding with a keycode

See the
[xkbcommon-keysyms.h header](https://github.com/xkbcommon/libxkbcommon/blob/master/include/xkbcommon/xkbcommon-keysyms.h)
for all the keysyms. The name you should use is the segment after `XKB_KEY_`.

If you want to bind by a keycode, you can put it in the KEY position with
a `code:` prefix, e.g.:

```lua
hl.bind("SUPER + code:28", hl.dsp.exec_cmd("amongus"))
```

This will bind <key>SUPER</key> + <key>t</key> since <key>t</key> is keycode 28.

> [!NOTE]
> If you are unsure of what your key's name or keycode is, you can use [`wev`](https://github.com/jwrdegoede/wev) to find out.

## Binding modkeys only

<!-- TODO: this is fixed with one of the newer commits, needs rewrtining https://github.com/hyprwm/Hyprland/pull/15568 -->

To only bind modkeys, you need to use the TARGET modmask (with the
activating mod) and the `release` flag, e.g.:

```lua
-- bind `exec amongus` to SUPER + ALT.
hl.bind("ALT + ALT_L", hl.dsp.exec_cmd("amongus"), { release = true })
```

## Multiple binds to one key

> [!WARNING]
> The keybinds will be executed top to bottom, in the order they were written in.

You can trigger multiple actions with the same keybind by using a lua lambda function, with different `disapatcher`s and `param`s:

```lua
-- To switch between windows in a floating workspace:
hl.bind("SUPER + Tab", function()
    -- Change focus to another window
    hl.dispatch(hl.dsp.window.cycle_next())
    -- Bring it to the top
    hl.dispatch(hl.dsp.window.bring_to_top())
end)
```

## Unbind

To unbind a key use `hl.unbind("key")` method.
Key in `hl.unbind` is case-sensitive and must exactly match the case of the `hl.bind` you are unbinding.

```lua
hl.bind("SUPER + TAB", hl.focus.workspace("e+1"))
hl.unbind("SUPER + Tab") -- this will NOT unbind
hl.unbind("SUPER + TAB") -- this will unbind
```

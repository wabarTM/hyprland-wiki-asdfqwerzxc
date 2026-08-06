---
title: Lua code snippets
weight: 20
---

## Code snippets

Code snippets provided on thing page may be useful for stealing 
some functions or getting inspiration.

<!-- TODO scrap discord/forums for helper functions that may or may not be useful -->

### Float browser's extension windows

All known browsers have the annoying "feature" of setting title to their own name when opening
extensions' window. This functiion listens to opened windows and floats the matches

```lua
hl.on("window.open", function(w)
    if w.class ~= "firefox" then return end
    if w.initial_title ~= "Mozilla Firefox" then return end

    local ff_windows = hl.get_windows({ class = "firefox" })
    if #ff_windows <= 1 then return end

    hl.dispatch(hl.dsp.window.float({ action = "set", window = w }))

    local sub
    sub = hl.on("window.title", function(tw)
        if tw.address ~= w.address then return end
        if tw.title == ""
            or tw.title == "Mozilla Firefox"
            or tw.title == "about:blank"
            or tw.title:match("^about:.*Mozilla Firefox$") then return end

        sub:remove()

        if tw.title:match("^Extension:") then
            hl.dispatch(hl.dsp.window.resize({ x = 800, y = 600, window = tw }))
            hl.dispatch(hl.dsp.window.center({ window = tw }))
            hl.dispatch(hl.dsp.focus({ window = tw }))
        else
            hl.dispatch(hl.dsp.window.float({ action = "unset", window = tw }))
        end
    end)
end)
```

### "Starts with" helper function

```lua
local function starts_with(string, prefix)
    return string:find(prefix, 1, true) == 1
end
```

### Helper function to make layout-dependent binds

```lua
local function layout_bind(bind_table)
        local unrolled_binds = {}
        for keys, action in pairs(bind_table) do
                for key in string.gmatch(keys, "[^,%s]+") do
                        unrolled_binds[key] = action
                end
        end

        local workspace = hl.get_active_special_workspace() or hl.get_active_workspace()

        if not workspace then; return; end

        local layout = workspace.tiled_layout

        if unrolled_binds[layout] then
                hl.dispatch(unrolled_binds[layout]);
        elseif unrolled_binds["default"] then
                hl.dispatch(unrolled_binds["default"]);
        end
end
```

Usage:

```lua
hl.bind("SUPER + H", function()
        layout_bind({
                ["scrolling"] = hl.dsp.layout("move -200"),
                ["default"] = hl.dsp.no_op(),
        })
end)

hl.bind("SUPER + L", function()
        layout_bind({
                ["scrolling"] = hl.dsp.layout("move +200"),
                ["default"] = hl.dsp.no_op(),
        })
end)
```

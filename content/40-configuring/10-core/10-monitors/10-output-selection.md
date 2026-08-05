---
title: Output selection
weight: 10
---

<!-- TODO this is ass, needs rewrite -->

For more specific rules, you can also use the output's description (see
`hyprctl monitors` for more details). If the output of `hyprctl monitors` looks
like the following:


```
Monitor eDP-1 (ID 0):
        1920x1080@60.00100 at 0x0
        description: Chimei Innolux Corporation 0x150C (eDP-1)
        make: Chimei Innolux Corporation
        model: 0x150C
        [...]
```

then the `description` value up to, but not including the portname `(eDP-1)` can
be used as the `output` field with a `desc:` prefix:

```lua
hl.monitor({ output = "desc:Chimei Innolux Corporation 0x150C", mode = "preferred", position = "auto", scale = 1.5 })
```

Remember to remove the `(portname)`!

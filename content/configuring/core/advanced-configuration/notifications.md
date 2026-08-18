---
weight: 30
title: Notifications
---

<!-- i cant think on how to write this page -->
<!-- please help. wabar.-->

Hyprland has simple built-in notification system.
Notifications are positioned in the top-right corner of the monitor.
**They are not meant to handle your system notifications.**

## Parameters

| Name | Description | Type | Default | Limits |
| --- | --- | --- | --- | --- |
| text | Text of the notification | str | [[Empty]] | |
| timeout | Timeout/duration in ms | int | [[Required]] | |
| icon | Icon type, see [below](#icon)| str | `"none"` | |
| color | Notification color. `0` means "default color for icon" | color | 0 | |
| font_size | Size of the font used to display the notification | int | 13 | |

### Icon

Icon can be on of:
<!-- NOTE: im not putting multiple icons here https://github.com/hyprwm/Hyprland/blob/9d4f7a83ce2764ddb51b4ea01f8ae1e6f1c18f66/src/notification/SharedDefines.hpp#L19 -->
| Icon name | Lua | hyprctl | Color |
| --- | --- | --- | --- |
| None | none | -1 | #000000 |
| Warning | warn/warning | 0 | #FFCC66 |
| Info | info | 1 | #80FFFF |
| Hint | hint | 2 | #B3FFCC |
| Error | err/error | 3 | #FF4D4D |
| Confused | confused/question | 4 | #FFCC99 |
| Ok | ok | 5 | #80FF80 |

## Creating a notification

### Lua

```lua
-- Create "HL.Notification" object and push the notification
hl.notification.create({ text = "Hello from Lua!", timeout = 15000, icon = "info", font_size = 20 })

-- Assign  notification object to "somevar" for later use and pushes the notification
local somevar = hl.notification.create({ text = "Please store me", timeout = 228638, icon = "hint" })

-- Get a table of all active notifications as "HL.Notification" objects
hl.notification.get()
```

### Hyprctl

To create a notification from hyprctl, you can use `hyprctl notify` command.
See more about it [here](./using-hyprctl#notify)

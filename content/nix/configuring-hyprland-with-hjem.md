---
title: Configuring Hyprland with Hjem
weight: 30
---

Hjem ("home" in Danish) is a module system that implements a simple and streamlined way to manage files in your $HOME, such as but not limited to files in your ~/.config. Hjem aims to serve as an alternative and easy-to-grasp utility for managing your $HOME purely and safely.

You can read the wonderful [Hjem documentation](https://hjem.feel-co.org/) to learn how to install and use it.

Example for configuring Hyprland using Hjem:

```lua {filename="hyprland.nix"}
{
    hjem.users.youruser.files.".config/hypr/hyprland.lua".text = ''
      # Your hyprland.lua content goes here.
      # You can use string interpolation for nix to evaluate variables at build time.
      # An example of an extract of a lua config using string interpolation for some variables to be interpreted by nix:
      -- Monitors
        -- Variables
          local monitor1 = "${config.custom.host.monitors."1"}"
          local monitor2 = "${config.custom.host.monitors."2"}"
          hl.monitor({
              output   = monitor2,
              mode     = "preferred",
              position = "auto-center-right",
              scale    = 1.0,
              ${lib.optionalString (hostName == "deck") "transform = 3"}
          })
    ''
}
```

You could also just source a hyprland.lua file using Hjem to link it into its place at .config/hypr/

---
weight: 80
title: Nix
---

## Installing Hyprland on Nixos

See [Hyprland on NixOS](./installing-hyprland-on-nixos).  

## Using Hyprland on any distro with the nix package manager

See [this page](./hyprland-on-any-distro-using-nix).

## Configuring Hyprland in NixOS

You need to choose one of the three ways for configuring hyprland in nix: hjem, home manager, or the upstream module.

### Hjem

Read [Configuring Hyprland with Hjem](./configuring-hyprland-with-hjem).

### Home Manager

Read [Configuring Hyprland with Home Manager](./configuring-hyprland-with-home-manager).

For the adventurous, [@spikespaz](https://github.com/spikespaz) has made a
Hyprland module that can be used in Home Manager and NixOS. It can be found
[here](https://github.com/hyprland-community/hyprnix).

### The upstream module

The [upstream module](https://github.com/hyprwm/Hyprland/blob/main/nix/module.nix)
provides options similar to the ones in the Home Manager module.

To use it, simply add

```nix
{inputs, ...}: {
  imports = [inputs.hyprland.nixosModules.default];

  programs.hyprland = {
    # usual Nixpkgs module options
    plugins = [
      #...
    ];
    settings = {
      # ...
    };
  };
}
```

## Advanced

The section [Advanced](./advanced) contains tweaks as options, overrides, overlays, building plugins with nix, ...

## Contributing and debugging Hyprland

See this page if you want to debug some issue or contribute code to hyprland: [Contributing and debugging](./contributing-and-debugging).

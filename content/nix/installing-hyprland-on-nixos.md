---
title: Hyprland on NixOS
weight: 10
---

You can install Hyprland from:
+ The nixpkgs repository if you want a proper released version from Hyprland.
+ The hyprland flake if you want the latest git commit available at the moment.

{{< tabs items="Nixpkgs repository,The Hyprland flake" >}}

{{< tab "Nixpkgs repository" >}}

```nix {filename="configuration.nix"}
{
  programs.hyprland.enable = true; # enable Hyprland
}
```

This will use the Hyprland version included in the Nixpkgs release you're using.

{{< /tab >}}

{{< tab "The Hyprland flake" >}}

> [!NOTE]
> If you don't want to compile Hyprland yourself, make sure to enable [Cachix](../Cachix).

In case you want to use the _development_ version of Hyprland, you can add it like
this:

```nix {filename="flake.nix"}
{
  inputs.hyprland.url = "github:hyprwm/Hyprland";
  # ...

  outputs = {nixpkgs, ...} @ inputs: {
    nixosConfigurations.HOSTNAME = nixpkgs.lib.nixosSystem {
      specialArgs = { inherit inputs; }; # this is the important part
      modules = [
        ./configuration.nix
      ];
    };
  };
}
```

Don't forget to change the `HOSTNAME` to your actual hostname!

```nix {filename="configuration.nix"}
{inputs, pkgs, ...}: {
  programs.hyprland = {
    enable = true;
    # set the flake package
    package = inputs.hyprland.packages.${pkgs.stdenv.hostPlatform.system}.hyprland;
    # make sure to also set the portal package, so that they are in sync
    portalPackage = inputs.hyprland.packages.${pkgs.stdenv.hostPlatform.system}.xdg-desktop-portal-hyprland;
  };
}
```

If you start experiencing lag and FPS drops in games or programs like Blender on
**stable** NixOS when using the Hyprland flake, it is most likely a `mesa`
version mismatch between your system and Hyprland.

You can fix this issue by using `mesa` from Hyprland's `nixpkgs` input:

```nix {filename="configuration.nix"}
{pkgs, inputs, ...}: let
  pkgs-unstable = inputs.hyprland.inputs.nixpkgs.legacyPackages.${pkgs.stdenv.hostPlatform.system};
in {
  hardware.graphics = {
    package = pkgs-unstable.mesa;

    # if you also want 32-bit support (e.g for Steam)
    enable32Bit = true;
    package32 = pkgs-unstable.pkgsi686Linux.mesa;
  };
}
```

For more details, see
[issue #5148](https://github.com/hyprwm/Hyprland/issues/5148).

{{< /tab >}}

{{< /tabs >}}

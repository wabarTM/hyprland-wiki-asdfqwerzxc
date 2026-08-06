---
weight: 20
title: Workspace Rules
---

You can set workspace rules to achieve workspace-specific behaviors. For
instance, you can define a workspace where all windows are drawn without borders
or gaps.

For layout-specific rules, see the specific layout page. For example:
[Master Layout->Workspace Rules](../../Layouts/Master-Layout#workspace-rules).

## Syntax

```lua
hl.workspace_rule(workspace, rule1, rule2, ...)
```

- WORKSPACE is a valid workspace identifier (see
  [Dispatchers->Workspaces](../Dispatchers#workspace)). This field is
  mandatory. This _can be_ a workspace selector, but please note
  workspace selectors can only match _existing_ workspaces.
- RULES is one (or more) rule(s) as described here in [rules](#rules).

## Rules

| Rule | Description | type |
| --- | --- | --- |
| animation | The animation style to use for this workspace. | string |
| monitor | Binds a workspace to a monitor. See [syntax](#syntax) and [Monitors](../Monitors). | string |
| default | Whether this workspace should be the default workspace for the given monitor | bool |
<<<<<<< HEAD:content/Configuring/Basics/Workspace-Rules.md
| gaps_in | Set the gaps between windows (equivalent to [General->gaps_in](../Variables#general)) | css_gaps | `5` |
| gaps_out | Set the gaps between windows and monitor edges (equivalent to [General->gaps_out](../Variables#general)) | css_gaps | `20` |
| float_gaps | Set the gaps for floating windows (equivalent to [General->float_gaps](../Variables#general)) | css_gaps | `0` |
=======
| float_gaps | Set the gaps for floating windows (equivalent to [General->float_gaps](../Variables#general)) | css_gaps | `0` |
| gaps_in | Set the gaps between windows (equivalent to [General->gaps_in](../Variables#general)) | css_gaps |
| gaps_out | Set the gaps between windows and monitor edges (equivalent to [General->gaps_out](../Variables#general)) | css_gaps |
>>>>>>> 9473901 (Big (huge) restructure):content/40-configuring/10-core/60-rules/20-workspace-rules.md
| border_size | Set the border size around windows (equivalent to [General->border_size](../Variables#general)) | int |
| no_border | Whether to disable borders | bool |
| no_shadow | Whether to disable shadows | bool |
| no_wobble | Whether to disable wobble | bool |
| no_rounding | Whether to disable rounded windows | bool |
| no_wobble | Whether to disable wobble | bool |
| decorate | Whether to draw window decorations or not | bool |
| persistent | Keep this workspace alive even if empty and inactive | bool |
| on_created_empty | A command to be executed once a workspace is created empty (i.e. not created by moving a window to it). See the [command syntax](../Dispatchers#executing-with-rules) | string |
| default_name | A default name for the workspace. | string |
| layout | The layout to use for this workspace. | string |
<<<<<<< HEAD:content/Configuring/Basics/Workspace-Rules.md
| animation | The animation style to use for this workspace. | string |
=======
>>>>>>> 9473901 (Big (huge) restructure):content/40-configuring/10-core/60-rules/20-workspace-rules.md
| layout_opts | A table of layout-specific options for this workspace. Keys and values depend on the layout. | table |

{{% details title="Examples" closed="true" %}}

```lua
hl.workspace_rule({ workspace = "3", no_rounding = true, decorate = false })
hl.workspace_rule({ workspace = "name:coding", no_rounding = true, decorate = false, gaps_in = 0, gaps_out = 0, no_border = true, monitor = "DP-1" })
hl.workspace_rule({ workspace = "8", border_size = 8 })
hl.workspace_rule({ workspace = "name:Hello", monitor = "DP-1", default = true })
hl.workspace_rule({ workspace = "name:gaming", monitor = "desc:Chimei Innolux Corporation 0x150C", default = true })
hl.workspace_rule({ workspace = "5", on_created_empty = "[float] firefox" })
hl.workspace_rule({ workspace = "special:scratchpad", on_created_empty = "foot" })
hl.workspace_rule({ workspace = "15", animation = "slidevert", default_name = "slider" })
```

{{% /details %}}

<!-- TODO move to code snippets -->
### Smart gaps

To replicate "smart gaps" / "no gaps when only" from other WMs/Compositors, use this bad boy:

```lua
hl.workspace_rule({ workspace = "w[tv1]", gaps_out = 0, gaps_in = 0 })
hl.workspace_rule({ workspace = "f[1]", gaps_out = 0, gaps_in = 0 })
hl.window_rule({ match = { float = false, workspace = "w[tv1]" }, border_size = 0 })
hl.window_rule({ match = { float = false, workspace = "w[tv1]" }, rounding = 0 })
hl.window_rule({ match = { float = false, workspace = "f[1]" }, border_size = 0 })
hl.window_rule({ match = { float = false, workspace = "f[1]" }, rounding = 0 })
```

#### Smart gaps (ignoring special workspaces)

You can combine workspace selectors for more fine-grained control, for example, to ignore special workspaces:

```lua
hl.workspace_rule({ workspace = "w[tv1]s[false]", gaps_out = 0, gaps_in = 0 })
hl.workspace_rule({ workspace = "f[1]s[false]", gaps_out = 0, gaps_in = 0 })
hl.window_rule({ match = { float = false, workspace = "w[tv1]s[false]" }, border_size = 0 })
hl.window_rule({ match = { float = false, workspace = "w[tv1]s[false]" }, rounding = 0 })
hl.window_rule({ match = { float = false, workspace = "f[1]s[false]" }, border_size = 0 })
hl.window_rule({ match = { float = false, workspace = "f[1]s[false]" }, rounding = 0 })
```

### Per-workspace layouts

Use workspace rules to set per-workspace layouts:

```lua
hl.workspace_rule({ workspace = "2", layout = "scrolling" })
```

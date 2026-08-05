---
weight: 20
title: Config options
---

<!-- TODO: rewrite -->
This page documents all the "options" of Hyprland. For binds, monitors,
animations, etc. see the sidebar.

Please keep in mind some options that are layout-specific will be documented in
the layout pages and not here. (See the Sidebar for Dwindle and Master layouts)

## Syntax

```lua
hl.config({
    category = {
        value = key
    },
    category2 = {
        value1 = key1,
        value2 = key2,
    },
})
```

You are allowed multiple `hl.config()` invocations, each one will update just what you pass into it.

This is completely valid:

```lua
function updateSomeVar()
    hl.config({ cat = { val = 12 } })
end
```

To use a dispatcher (any of `hl.dsp.*`) inside a function, you need to wrap it in `hl.dispatch()` for it to be executed properly. For example:

```lua
hl.bind("ALT + Tab", function()
    hl.dsp.window.cycle_next()
    hl.dsp.window.bring_to_top()
end)
```

will not work since `hl.bind` runs the `function()` and not `hl.dsp.*`'s. The working example is:

```lua
hl.bind("ALT + Tab", function()
    hl.dispatch(hl.dsp.window.cycle_next())
    hl.dispatch(hl.dsp.window.bring_to_top())
end)
```

## Sections

### General

| Name | Description | Type | Default |
|---|---|---|---|
| allow_tearing | Master switch for allowing tearing to occur. See the [Tearing page](../../40-extra/40-tearing). | bool | `false` |
| border_size | Size of the border around windows | int | `1` |
| gaps_in | Gaps between windows | css_gaps | `5` |
| gaps_out | Gaps between windows and monitor edges | css_gaps | `20` |
| gaps_workspaces | Gaps between workspaces. Stacks with gaps_out. | int | `0` |
| float_gaps | Gaps between windows and monitor edges for floating windows `-1` means default | css_gaps | `0` |
| resize_corner | Force floating windows to use a specific corner when being resized (1-4 going clockwise from top left, 0 to disable) | int | `0` |
| resize_on_border | Enables resizing windows by clicking and dragging on borders and gaps | bool | `false` |
| extend_border_grab_area | Extends the area around the border where you can click and drag on, only used when `general.resize_on_border` is on. | int | `15` |
| hover_icon_on_border | Show a cursor icon when hovering over borders, only used when `general.resize_on_border` is on. | bool | `true` |
| layout | Which layout to use. Options: `"dwindle"`/`"master"`/`"scrolling"`/`"monocle"` | str | `"dwindle"` |
| locale | Overrides the system locale, e.g. `"en_US"`, `"es"` | str | \[\[Empty\]\] |
| modal_parent_blocking | Whether parent windows of modals will be interactive | bool | `true` |
| no_focus_fallback | If true, will not fall back to the next available window when moving focus in a direction where no window was found | bool | `false` |


_Subcategory `general.col.`_

| name | description | type | default |
| --- | --- | --- | --- |
| active_border | Border color for the active window | gradient | `0xffffffff` |
| inactive_border | Border color for inactive windows | gradient | `0xff444444` |
| nogroup_border | Inactive border color for window that cannot be added to a group (see `hl.dsp.window.deny_from_group` dispatcher) | gradient | `0xffffaaff` |
| nogroup_border_active | Active border color for window that cannot be added to a group | gradient | `0xffff00ff` |

#### Snap

_Subcategory `general.snap.`_

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | enable snapping for floating windows | bool | `false` |
| border_overlap | if true, windows snap such that only one border's worth of space is between them | bool | `false` |
| respect_gaps | if true, snapping will respect gaps between windows(set in general:gaps_in) | bool | `false` |
| monitor_gap | minimum gap in pixels between window and monitor edges before snapping | int | `10` |
| window_gap | minimum gap in pixels between windows before snapping | int | `10` |

### Decoration

| name | description | type | default |
| --- | --- | --- | --- |
| border_part_of_window | whether the window border should be a part of the window | bool | `true` |
| active_opacity | opacity of active windows. [0.0 - 1.0] | float | `1.0`|
| inactive_opacity | opacity of inactive windows. [0.0 - 1.0] | float | `1.0` |
| fullscreen_opacity | opacity of fullscreen windows. [0.0 - 1.0] | float | `1.0` |
| rounding | rounded corners' radius (in layout px) | int | `0` |
| rounding_power | adjusts the curve used for rounding corners, larger is smoother, 2.0 is a circle, 4.0 is a squircle, 1.0 is a triangular corner. [1.0 - 10.0] | float | `2.0` |
| dim_around | how much the `dim_around` window rule should dim by. [0.0 - 1.0] | float | `0.4` |
| dim_inactive | enables dimming of inactive windows | bool | `false` |
| dim_modal | enables dimming of parents of modal windows | bool | `true` |
| dim_special | how much to dim the rest of the screen by when a special workspace is open. [0.0 - 1.0] | float | `0.2 `|
| dim_strength | how much inactive windows should be dimmed [0.0 - 1.0] | float | `0.5` |
| screen_shader | a path to a custom shader to be applied at the end of rendering. See `examples/screenShader.frag` for an example. | str | \[\[Empty\]\] |

#### Blur

_Subcategory `decoration.blur.`_

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | Enable kawase window background blur | bool | `true` |
| brightness | Brightness modulation for blur. [0.0 - 2.0] | float | `1` |
| contrast | Contrast modulation for blur. [0.0 - 2.0] | float | `0.8916` |
| ignore_opacity | Make the blur layer ignore the opacity of the window | bool | `true` |
| input_methods | Whether to blur input methods, e.g. `fcitx5` | bool | `false` |
| input_methods_ignorealpha | Works like ignore_alpha in layer rules. If pixel opacity is below set value, will not blur. [0.0 - 1.0] | float | `0.2` |
| new_optimizations | Whether to enable further optimizations to the blur. Recommended to leave on, as it will massively improve performance. | bool | `true` |
| noise | How much noise to apply. [0.0 - 1.0] | float | `0.0117` |
| passes | The amount of passes to perform | int | `1` |
| popups | Whether to blur popups, e.g. `right-click menus` | bool | `false` |
| popups_ignorealpha | Works like ignore_alpha in layer rules. If pixel opacity is below set value, will not blur. [0.0 - 1.0] | float | `0.2` |
| size | Blur size (distance) | int | `8` |
| special | Whether to blur behind the special workspace (note: expensive) | bool | `false` |
| vibrancy | Increase saturation of blurred colors. [0.0 - 1.0] | float | `0.1696` |
| vibrancy_darkness | How strong the effect of `vibrancy` is on dark areas. [0.0 - 1.0] | float | `0.0` |
| xray | If enabled, floating windows will ignore tiled windows in their blur. Only available if new_optimizations is true. Will reduce overhead on floating blur significantly. | bool | `false` |


> [!NOTE]
> `blur.size` and `blur.passes` have to be at least 1.
> 
> Increasing `blur.passes` is necessary to prevent blur looking wrong on higher
> `blur.size` values, but remember that higher `blur.passes` will require more
> strain on the GPU.

#### Shadow

_Subcategory `decoration.shadow.`_

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | Enable drop shadows on windows | bool | `true` |
| color | Active window shadow's color. Alpha dictates the opacity. | color/gradient | `0xee1a1a1a` |
| color_inactive | Inactive window shadow's color. If not set, will fall back to `color` | color/gradient | unset |
| offset | Shadow's rendering offset. | vec2 | `{0, 0}` |
| range | Shadow range (size) in pixels | int | `4` |
| render_power | In what power to render the falloff. More power, the faster the falloff. Options: [1 - 4] | int | `3` |
| scale | Shadow's scale. Options: [0.0 - 1.0] | float | `1.0` |
| sharp | If enabled, will make the shadows sharp, akin to an infinite render power | bool | `false` |


#### Glow

_Subcategory `decoration.glow.`_

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | Enable inner glow on windows | bool | `false` |
| range | Glow range (size) in pixels | int | `10` |
| render_power | In what power to render the falloff. More power, the faster the falloff. Options: [1 - 4] | int | `3` |
| color | Active window glow's color. Alpha dictates opacity. | color | `0xee1a1a1a` |
| color_inactive | Inactive window glow's color. If not set, will fall back to `color` | color | unset |

#### Motion blur

_Subcategory `decoration.motion_blur.`_

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | enable motion blur on moving / resizing windows | bool | `false` |
| samples | The amount of samples to render. More will mean clearer blur, at the cost of more compute. | int | `7` |

#### Wobble

_Subcategory `decoration.wobble.`_

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | enable wobble on moving / resizing windows | bool | `false` |
| mesh | amount of wobble mesh vertices per edge | int | `12` |
| stiffness | spring stiffness for wobble deformation | float | `200` |
| damping | spring damping for wobble deformation | float | `12` |
| mass | spring mass for wobble deformation | float | `1` |
| intensity | wobble deformation impulse multiplier | float | `0.2` |
| value_epsilon | position epsilon below which wobble is considered stable | float | `0.25` |
| velocity_epsilon | velocity epsilon below which wobble is considered stable | float | `2` |

### Animations

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | Enable animations | bool | `true` |
| workspace_wraparound | Enable workspace wraparound, causing directional workspace animations to animate as if the first and last workspaces were adjacent | bool | `false` |

> [!NOTE]
> _[More about Animations](../../Advanced-and-Cool/Animations)._

### Input

| name | description | type | default |
|---|---|---|---|
| kb_model | Appropriate XKB keymap parameter. See the note [below](#xkb-keymap-params). | str | \[\[Empty\]\] |
| kb_layout | Appropriate XKB keymap parameter | str | `"us"` |
| kb_variant | Appropriate XKB keymap parameter | str | \[\[Empty\]\] |
| kb_options | Appropriate XKB keymap parameter | str | \[\[Empty\]\] |
| kb_rules | Appropriate XKB keymap parameter | str | \[\[Empty\]\] |
| kb_file | If you prefer, you can use a path to your custom .xkb file. | str | \[\[Empty\]\] |
| numlock_by_default | Enable numlock by default. | bool | `false` |
| resolve_binds_by_sym | Determines how keybinds act when multiple layouts are used. If false, keybinds will always act as if the first specified layout is active. If true, keybinds specified by symbols are activated when you type the respective symbol with the current layout. | bool | `false` |
| repeat_rate | The repeat rate for held-down keys, in repeats per second. | int | `25` |
| repeat_delay | Delay before a held-down key is repeated, in milliseconds. | int | `600` |
| sensitivity | Sets the mouse input sensitivity. Additional info: [libinput#pointer-acceleration](https://wayland.freedesktop.org/libinput/doc/latest/pointer-acceleration.html#pointer-acceleration). Options: [-1.0 - 1.0] | float | `0.0` |
| accel_profile | Sets the cursor acceleration profile. See the note [below](#accel-profile). Leave empty to use `libinput`'s default mode for your input device. [libinput#pointer-acceleration](https://wayland.freedesktop.org/libinput/doc/latest/pointer-acceleration.html#pointer-acceleration). Options: `"adaptive"`/`"flat"`/`"custom"`| str | \[\[Empty\]\] |
| force_no_accel | Force no cursor acceleration. This bypasses most of your pointer settings to get as raw of a signal as possible. **Enabling this is not recommended due to potential cursor desynchronization.** | bool | `false` |
| rotation | Sets the rotation of a device in degrees clockwise off the logical neutral position. Options: [0 - 359] | int | `0` |
| left_handed | Switches RMB and LMB | bool | `false` |
| scroll_points | Sets the scroll acceleration profile, when `accel_profile` is set to `"custom"`. Has to be in the form `"<step> <points>"`. Leave empty to have a flat scroll curve. | str | \[\[Empty\]\] |
| scroll_method | Sets the scroll method. Additional info: [libinput#scrolling](https://wayland.freedesktop.org/libinput/doc/latest/scrolling.html). Options: `"2fg"`/`"edge"`/`"on_button_down"`/`"no_scroll"` (2fg - 2 fingers) | str | \[\[Empty\]\] |
| scroll_button | Sets the scroll button. Check `wev` for the ID. `0` means default. | int | `0` |
| scroll_button_lock | If the scroll button lock is enabled, the button does not need to be held down. Pressing and releasing the button toggles the button lock, which logically holds the button down or releases it. While the button is logically held down, motion events are converted to scroll events. | bool | `false` |
| scroll_factor | Multiplier added to scroll movement for external mice. Note that there is a separate setting for [touchpad scroll_factor](#touchpad).  | float | `1.0` |
| natural_scroll | Inverts scrolling direction. When enabled, scrolling moves content directly, rather than manipulating a scrollbar. | bool | `false` |
| follow_mouse | Specify if and how cursor movement should affect window focus. See the note [below](#follow-mouse). Options: [0 - 3] | int | `1` |
| follow_mouse_shrink | Shrinks the inactive window hitboxes used for focus detection by the specified number of pixels. This creates a dead zone in gaps between windows where moving the cursor will not change focus. Works only with `follow_mouse` set to `1`. | int | `0` |
| follow_mouse_threshold | The smallest distance in logical pixels the mouse needs to travel for the window under it to get focused. Works only with `follow_mouse` set to`1`. | float | `0.0` |
| focus_on_close | Controls the window focus behavior when a window is closed. When set to `0`, focus will shift to the next window candidate. When set to `1`, focus will shift to the window under the cursor. When set to `2`, focus will shift to the most recently used/active window. Options: [0 - 2] | int | `0` |
| mouse_refocus | If enabled, mouse focus will switch to the hovered window when the pointer crosses a window boundary. Works only with `follow_mouse` set to `1`. | bool | `true` |
| float_switch_override_focus | If `1`/`2` focus will change to the window under the cursor when changing from tiled-to-floating and vice versa. If `2`, focus will also follow mouse on float-to-float switches. `0` means disabled. Options: [0 - 2]| int | `1` |
| special_fallthrough | if enabled, having only floating windows in the special workspace will not block focusing windows in the regular workspace. | bool | `false` |
| off_window_axis_events | Handles axis events around (gaps/border for tiled, dragarea/border for floated) a focused window. `0` ignores axis events `1` sends out-of-bound coordinates `2` fakes pointer coordinates to the closest point inside the window `3` warps the cursor to the closest point inside the window. Options: [0 - 3] | int | `1` |
| emulate_discrete_scroll | Emulates discrete scrolling from high resolution scrolling events. `0` disables it, `1` enables handling of non-standard events only, and `2` force enables all scroll wheel events to be handled. Options: [0 - 2] | int | `1` |

> [!NOTE] **XKB Settings**
> ##### xkb keymap params
>
> You can find a list of models, layouts, variants and options in
> [`/usr/share/X11/xkb/rules/evdev.lst`](file:///usr/share/X11/xkb/rules/evdev.lst).
> Alternatively, you can use the `localectl` command to discover what is available
> on your system.
> 
> For switchable keyboard configurations, take a look at
> [the binds page entry](../50-binds/20-keyboard-layouts).

> [!NOTE] Follow Mouse Cursor
> ##### follow mouse
> - 0 - Cursor movement will not change focus.
> - 1 - Cursor movement will always change focus to the window under the cursor.
> - 2 - Cursor focus will be detached from keyboard focus. Clicking on a window
>   will move keyboard focus to that window.
> - 3 - Cursor focus will be completely separate from keyboard focus. Clicking on
>   a window will not change keyboard focus.

> [!NOTE] **Custom Accel Profiles**
> 
> ##### accel profile
> 
> `custom <step> <points...>`
> 
> Example: `custom 200 0.0 0.5`
> 
> ##### scroll points
>  
> `<step> <points...>`
> 
> Example: `0.2 0.0 0.5 1 1.2 1.5`
> 
> To mimic the Windows acceleration curves, take a look at
> [this script](https://gist.github.com/fufexan/de2099bc3086f3a6c83d61fc1fcc06c9).
> 
> See
> [the libinput doc](https://wayland.freedesktop.org/libinput/doc/latest/pointer-acceleration.html)
> for more insights on how it works.

#### Touchpad

_Subcategory `input.touchpad.`_

| name | description | type | default |
| --- | --- | --- | --- |
| clickfinger_behavior | Button presses with 1, 2, or 3 fingers will be mapped to LMB, RMB, and MMB respectively. This disables interpretation of clicks based on location on the touchpad. Additional info: [libinput#clickfinger-behavior](https://wayland.freedesktop.org/libinput/doc/latest/clickpad-softbuttons.html#clickfinger-behavior) | bool | `false` |
| disable_while_typing | Disable the touchpad while typing. | bool | `true` |
| drag_3fg | Enables three finger drag. Additional info: [libinput#drag-3fg](https://wayland.freedesktop.org/libinput/doc/latest/drag-3fg.html). Options: [0 - 2] | int | `0` |
| drag_lock | When enabled, lifting the finger off while dragging will not drop the dragged item. 0 -> disabled, 1 -> enabled with timeout, 2 -> enabled sticky. Additional info: [libinput#tap-and-drag](https://wayland.freedesktop.org/libinput/doc/latest/tapping.html#tap-and-drag). Options: [0 - 2] | int | `0` |
| flip_x | Inverts the horizontal movement of the touchpad | bool | `false` |
| flip_y | Inverts the vertical movement of the touchpad | bool | `false` |
| middle_button_emulation | Sending LMB and RMB simultaneously will be interpreted as a middle click. This disables any touchpad area that would normally send a middle click based on location. Additional info: [libinput#middle-button-emulation](https://wayland.freedesktop.org/libinput/doc/latest/middle-button-emulation.html) | bool | `false` |
| natural_scroll | Inverts scrolling direction. When enabled, scrolling moves content directly, rather than manipulating a scrollbar. | bool | `false` |
| scroll_factor | Multiplier applied to the amount of scroll movement. | float | `1.0` |
| tap_and_drag | Sets the tap and drag mode for the touchpad | bool | `true` |
| tap_button_map | Sets the tap button mapping for touchpad button emulation. When empty, defaults to `"lrm"`. L - Left, M - Middle, R - Right. Options: `"lrm"`/`"lmr"` | str | \[\[Empty\]\] |
| tap_to_click | Tapping on the touchpad with 1, 2, or 3 fingers will send LMB, RMB, and MMB respectively. | bool | `true` |

<!-- TODO check lrm/lmr thing if it really defaults to noting -->

#### Touchdevice

_Subcategory `input.touchdevice.`_

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | Whether input is enabled for touch devices. | bool | `true` |
| output | The monitor to bind touch devices. The default is auto-detection. To stop auto-detection, use an empty string. | string | \[\[Auto\]\] |
| transform | Transform the input from touchdevices. The possible transformations are the same as [those of the monitors](../10-monitors/20-positioning#rotation). | int | `0` |

#### Virtualkeyboard

_Subcategory `input.virtualkeyboard.`_

| name | description | type | default |
| --- | --- | --- | --- |
| release_pressed_on_close | Release all pressed keys by virtual keyboard on close. | bool | `false` |
| share_states | Unify key down states and modifier states with other keyboards. 0 -> no, 1 -> yes, 2 -> yes unless IME client | int | `2` |

#### Tablet

_Subcategory `input.tablet.`_

| name | description | type | default |
| --- | --- | --- | --- |
| output | The monitor to bind tablets. Can be `"current"` or a monitor name. Leave empty to map across all monitors. | string | \[\[Empty\]\] |
| transform | Transform the input from tablets. The possible transformations are the same as [those of the monitors](../10-monitors/20-positioning#rotation). | int | `0` |
| absolute_region_position | Whether to treat the `region_position` as an absolute position in monitor layout. Only applies when `output` is empty. | bool | `false` |
| active_area_position | Position of the active area in mm | vec2 | `{0, 0}` |
| active_area_size | Size of tablet's active area in mm | vec2 | `{0, 0}` |
| left_handed | If enabled, the tablet will be rotated 180 degrees | bool | `false` |
| region_position | Position of the mapped region in monitor layout relative to the top left corner of the bound monitor or all monitors. | vec2 | `{0, 0}` |
| region_size | Size of the mapped region. When this variable is set, tablet input will be mapped to the region. `{0, 0}` or invalid size means unset. | vec2 | `{0, 0}` |
| relative_input | Whether the input should be relative | bool | `false` |

#### Tablettool

_Subcategory `input.tablettool.`_

| name | description | type | default |
| --- | --- | --- | --- |
| eraser_button_mode | Change the eraser button behavior on the tool. When set to `0`, use the default hardware behavior of the tool. When set to `1`, the eraser button on the tool sends a button event instead. Options: [0 - 1] | int | 0 |
| eraser_button_override | Set a button to be button event when eraser_button_mode is set to `1`. Must be a valid button (e.g. BTN_STYLUS) excluding fake buttons (e.g. BTN_TOOL_\*) and keys (KEY_\*). Check `wev` for the ID. `0` means default | int | 0 |
| pressure_range_min | Set the minimum pressure range for the tool. Negative value means it will use device default's. Usually it is `0.0` | float | -1.0 |
| pressure_range_max | Set the maximum pressure range for the tool. Negative value means it will use device default's. Usually it is `1.0` | float | -1.0 |

### Per-device input config

Described [here](../30-devices).

### Gestures

_Subcategory `gestures.`_

| name | description | type | default |
| --- | --- | --- | --- |
| close_max_timeout | The timeout for a window to close when using a 1:1 gesture, in ms | int | `1000` |
| workspace_swipe_cancel_ratio | How much the swipe has to proceed in order to commence it. Example, when set to `0.7`: if more than 70% of the distance is covered - switch, else - cancel the gesture. Options: [0.0 - 1.0] | float | `0.5` |
| workspace_swipe_create_new | Whether a swipe right on the last workspace should create a new one. | bool | `true` |
| workspace_swipe_direction_lock | If enabled, switching direction will be locked when you swipe past the `direction_lock_threshold` (touchpad only). | bool | `true` |
| workspace_swipe_direction_lock_threshold | In pixels, the distance to swipe before direction lock activates (touchpad only). | int | `10` |
| workspace_swipe_distance | In pixels, the distance of the touchpad gesture | int | `300` |
| workspace_swipe_forever | If enabled, swiping will not clamp at the neighboring workspaces but continue to the further ones. | bool | `false` |
| workspace_swipe_invert | Invert the swipe direction (touchpad only) | bool | `true` |
| workspace_swipe_min_speed_to_force | Minimum speed in pixels per timepoint to force the change ignoring `cancel_ratio`. `0` means disabled | int | `30` |
| workspace_swipe_touch | Enable workspace swiping from the edge of a touchscreen | bool | `false` |
| workspace_swipe_touch_invert | Invert the swipe direction (touchscreen only) | bool | `false` |
| workspace_swipe_use_r | If enabled, swiping will use the `r` prefix instead of the `m` prefix for finding workspaces. | bool | `false` |

#### Scrolling

_Subcategory `gestures.scrolling.`_

| name | description | type | default |
| --- | --- | --- | --- |
| move_snap_to_grid | when releasing the scroll move gesture, whether it should try to snap to the grid | bool | `true` |
| move_snap_cursor | when releasing the scroll move gesture, whether it should snap the cursor to the newly focused window | bool | `true` |


### Group

<!-- TODO: what on earth groups are? I.e.: "Groups are for hyprland the same as Tabs are for firefox" -->

_Subcategory `group.`_

| name | description | type | default |
| --- | --- | --- | --- |
| auto_group | Whether new windows will be automatically grouped into the focused unlocked group. Note: if you want to disable auto_group only for specific windows, use [the "group barred" window rule](../60-rules/10-window-rules/#group-window-rule-options) instead. | bool | `true` |
| col.border_active | Active group border color | gradient | `0x66ffff00` |
| col.border_inactive | Inactive group border color | gradient | `0x66777700` |
| col.border_locked_active | Active locked group border color | gradient | `0x66ff5500` |
| col.border_locked_inactive | Inactive locked group border color | gradient | `0x66775500` |
| drag_into_group | Whether dragging a window into a unlocked group will merge them. `0` -> disabled, `1` -> enabled, `2` -> only when dragging into a groupbar. Options: [0 - 2] | int | `1` |
| focus_removed_window | Whether Hyprland should focus on the window that has just been moved out of the group | bool | `true` |
| group_on_movetoworkspace | Whether using hl.dsp.window.move({ workspace }) will merge the window into the workspace's solitary unlocked group | bool | `false` |
| insert_after_current | Whether new windows in a group spawn after current or at group tail | bool | `true` |
| merge_floated_into_tiled_on_groupbar | Whether dragging a floating window into a tiled window groupbar will merge them | bool | `false` |
| merge_groups_on_drag | Whether window groups can be dragged into other groups | bool | `true` |
| merge_groups_on_groupbar | Whether one group will be merged with another when dragged into its groupbar | bool | `true` |

#### Groupbar

_Subcategory `group.groupbar.`_

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | Enables groupbars | bool | `true` |
| blur | Applies blur to the groupbar indicators and gradients | bool | `false` |
| col.active | Active group bar background color | gradient | `0x66ffff00` |
| col.inactive | Inactive (out of focus) group bar background color | gradient | `0x66777700` |
| col.locked_active | Active locked group bar background color | gradient | `0x66ff5500` |
| col.locked_inactive | Inactive locked group bar background color | gradient | `0x66775500` |
| disable_when_only | Disable groupbar if it contains a single window | bool | `false` |
| font_family | Font used to display groupbar titles, use `misc.font_family` if not specified | string | \[\[Empty\]\] |
| font_size | Font size of groupbar title | int | `8` |
| font_weight_active | Font weight of active groupbar title | font_weight | `"normal"` |
| font_weight_inactive | Font weight of inactive groupbar title | font_weight | `"normal"` |
| gaps_in | Gap size between gradients | int | `2` |
| gaps_out | Gap size between gradients and window | int | `2` |
| gradients | Enables gradients | bool | `false` |
| gradient_round_only_edges | Round only the gradient edges of the entire groupbar | bool | `true` |
| gradient_rounding | How much to round the gradients | int | `2` |
| gradient_rounding_power | Adjusts the curve used for rounding gradient corners, larger is smoother, 2.0 is a circle, 4.0 is a squircle, 1.0 is a triangular corner. Options: [1.0 - 10.0] | float | `2.0` |
| height | Height of the groupbar | int | `14` |
| indicator_gap | Height of gap between groupbar indicator and title | int | `0` |
| indicator_height | Height of the groupbar indicator | int | `3` |
| keep_upper_gap | Add or remove upper gap | bool | `true` |
| middle_click_close | Whether middle clicking the groupbar closes the clicked window | bool | `true` |
| priority | Sets the decoration priority for groupbars | int | `3` |
| render_titles | Whether to render titles in the group bar decoration | bool | `true` |
| round_only_edges | Round only the indicator edges of the entire groupbar | bool | `true` |
| rounding | How much to round the indicator | int | `1` |
| rounding_power | Adjusts the curve used for rounding groupbar corners, larger is smoother, 2.0 is a circle, 4.0 is a squircle, 1.0 is a triangular corner. Options: [1.0 - 10.0] | float | `2.0` |
| scrolling | Whether scrolling in the groupbar changes group active window | bool | `true` |
| stacked | Render the groupbar as a vertical stack | bool | `false` |
| text_color | Color for window titles in the groupbar | color | `0xffffffff` |
| text_color_inactive | Color for inactive windows' titles in the groupbar (if unset, defaults to text_color) | color | unset |
| text_color_locked_active | Color for the active window's title in a locked group (if unset, defaults to text_color) | color | unset |
| text_color_locked_inactive | Color for inactive windows' titles in locked groups (if unset, defaults to text_color_inactive) | color | unset |
| text_offset | Adjust vertical position for titles | int | `0` |
| text_padding | Set horizontal padding for titles | int | `0` |

### Misc

_Subcategory `misc.`_

| name | description | type | default |
|---|---|---|---|
| allow_session_lock_restore | If true, will allow you to restart a lockscreen app in case it crashes | bool | `false` |
| always_follow_on_dnd | Will make mouse focus follow the mouse when drag and dropping. Recommended to leave it enabled, especially for people using focus follows mouse at 0. | bool | `true` |
| animate_manual_resizes | If true, will animate manual window resizes/moves | bool | `false` |
| animate_mouse_windowdragging | If true, will animate windows being dragged by mouse, note that this can cause weird behavior on some curves | bool | `false` |
| anr_missed_pings | Number of missed pings before showing the ANR dialog | int | `5` |
| background_color | Change the background color (requires enabled `disable_hyprland_logo`) | color | `0x111111` |
| bell_sound | path to custom wav/ogg system bell. "none" or an empty string mute it. "default" uses the system's current one. | str | `"default"` |
| close_special_on_empty | Close the special workspace if the last window is removed | bool | `true` |
| col.splash | Changes the color of the splash text (requires a monitor reload to take effect). | color | `0x55ffffff` |
| splash_font_family | Changes the font used to render the splash text, selected from system fonts (requires a monitor reload to take effect). | string | \[\[Empty\]\] |
| disable_autoreload | If true, the config will not reload automatically on save, and instead needs to be reloaded with `hyprctl reload`. Might save on battery. | bool | `false` |
| disable_hyprland_logo | Disables the random Hyprland logo / anime girl background. :( | bool | `false` |
| disable_hyprland_guiutils_check | Disable the warning if hyprland-guiutils is not installed | bool | `false` |
| disable_scale_notification | Disables notification popup when a monitor fails to set a suitable scale | bool | `false` |
| disable_splash_rendering | Disables the Hyprland splash rendering (requires a monitor reload to take effect) | bool | `false` |
| disable_watchdog_warning | Disables the warning about not using start-hyprland | bool | `false` |
| disable_xdg_env_checks | Disable the warning if XDG environment is externally managed | bool | `false` |
| enable_anr_dialog | Whether to enable the ANR (app not responding) dialog when your apps hang | bool | `true` |
| enable_swallow | Enable window swallowing | bool | `false` |
| screencopy_force_8b | Forces 8 bit screencopy | bool | `true` |
| swallow_regex | The _class_ regex to be used for windows that should be swallowed (usually, a terminal). To know more about the list of regex which can be used [use this cheatsheet](https://github.com/ziishaned/learn-regex/blob/master/README.md). | str | \[\[Empty\]\] |
| swallow_exception_regex | The _title_ regex to be used for windows that should _not_ be swallowed by the windows specified in swallow_regex, e.g. `wev`. The regex is matched against the parent, e.g. Kitty, window's title on the assumption that it changes to whatever process it's running. | str | \[\[Empty\]\] |
| exit_window_retains_fullscreen | If true, closing a fullscreen window makes the next focused window fullscreen | bool | `false` |
| focus_on_activate | Whether Hyprland should focus an app that requests to be focused (an `activate` request) | bool | `false` |
| float_force_onscreen | whether/how existing floating windows should be constrained to stay on-screen. 0 - no constraints, 1 - must be partially onscreen, 2 - must be fully onscreen [0/1/2] | int | `0` |
| new_float_force_onscreen | same as `float_force_onscreen`, but specifically for newly-spawned floating windows [0/1/2] | int | `2` |
| font_family | Set the global default font to render the text including debug fps/notification, config error messages and etc., selected from system fonts. | string | `"Sans"` |
| force_default_wallpaper | Enforce any of the 3 default wallpapers. 0 -> disables the anime background, 1 -> disables the anime background, 2 -> enables anime background, -1 -> random. | int | `-1` |
| initial_workspace_tracking | If enabled, windows will open on the workspace they were invoked on. 0 -> disabled, 1 -> single-shot, 2 -> persistent (all children too) | int | `1` |
| key_press_enables_dpms | If DPMS is set to off, wake up the monitors if a key is pressed. | bool | `false` |
| layers_hog_keyboard_focus | If true, will make keyboard-interactive layers keep their focus on mouse move, e.g. `wofi`, `bemenu` | bool | `true` |
| lockdead_screen_delay | Delay after which the "lockdead" screen will appear in case a lockscreen app fails to cover all the outputs (5 seconds max) | int | `1000` |
| middle_click_paste | Whether to enable middle-click-paste (aka primary selection) | bool | `true` |
| mouse_move_enables_dpms | If DPMS is set to off, wake up the monitors if the mouse moves. | bool | `false` |
| mouse_move_focuses_monitor | Whether mouse moving into a different monitor should focus it | bool | `true` |
| name_vk_after_proc | Name virtual keyboards after the processes that create them, e.g. `/usr/bin/fcitx5` will have hl-virtual-keyboard-fcitx5. | bool | `true` |
| on_focus_under_fullscreen | If there is a fullscreen or maximized window, decide whether a tiled window requested to focus should replace it, stay behind or disable the fullscreen/maximized state. 0 -> ignore focus request (keep focus on fullscreen window), 1 -> takes over, 2 -> unfullscreen/unmaximize | int | `2` |
| render_unfocused_fps | The maximum limit for render_unfocused windows' fps in the background (see also [Window-Rules](../60-rules/10-window-rules/#dynamic-effects), e.g. `render_unfocused`) | int | `15` |
| session_lock_blur | Enables blur for lockscreen. `session_lock_xray` must be enabled. | bool | `false` |
| session_lock_xray | If true, keep rendering workspaces below your lockscreen | bool | `false` |
| size_limits_tiled | Whether to apply min_size and max_size rules to tiled windows | bool | `false` |
| vrr | Controls the VRR (Adaptive Sync) of your monitors. 0 -> off, 1 -> on, 2 -> fullscreen only, 3 -> fullscreen with `video` or `game` content type | int | `0` |


### Layout

_Subcategory `layout.`_

| name | description | type | default |
|---|---|---|---|
| single_window_aspect_ratio | Whenever only a single window is shown on a screen, add padding so that it conforms to the specified aspect ratio. A value like `4 3` on a 16:9 screen will make it a 4:3 window in the middle with padding to the sides. | vec2 | `{0, 0}` |
| single_window_aspect_ratio_tolerance | Sets a tolerance for `single_window_aspect_ratio`, so that if the padding that would have been added is smaller than the specified fraction of the height or width of the screen, it will not attempt to adjust the window size. Options: [0.0 - 1.0] | float | `0.1` | 

### Binds

_Subcategory `binds.`_

| name | description | type | default |
| --- | --- | --- | --- |
| allow_pin_fullscreen | If enabled, allow fullscreen to pinned windows, and restore their pinned status afterwards | bool | `false` |
| allow_workspace_cycles | If enabled, workspaces don't forget their previous workspace, so cycles can be created by switching to the first workspace in a sequence, then endlessly going to the previous workspace | bool | `false` |
| disable_keybind_grabbing | If enabled, apps that request keybinds to be disabled, e.g. `VMs`, will not be able to do so | bool | `false` |
| drag_threshold | Movement threshold in pixels for window dragging and `click`/`drag` bind flags. `0` means disabled. | int | `0` |
| focus_preferred_method | Sets the preferred focus finding method when using `hl.dsp.focus({ direction })`/`hl.dsp.window.move({ direction })`/etc. `0` - most recent active window have priority, `1` - longer shared edges have priority). Options: [0 - 1] | int | `0` |
| hide_special_on_workspace_change | If enabled, changing the active workspace (including to itself) will hide the special workspace on the monitor where the newly active workspace resides | bool | `false` |
| ignore_group_lock | If enabled, dispatchers like `hl.dsp.window.move({ into_group })` and `hl.dsp.window.move({ out_of_group })` will ignore lock per group | bool | `false` |
| movefocus_cycles_fullscreen | If enabled, when on a fullscreen window, `hl.dsp.focus({ direction })` will cycle fullscreen, else, it will move the focus in a direction | bool | `false` |
| movefocus_cycles_groupfirst | If enabled, when in a grouped window, `hl.dsp.focus({ direction })` will cycle windows in the groups first, then at each ends of tabs, it'll move on to other windows/groups | bool | `false` |
| pass_mouse_when_bound | If enabled, will pass the mouse events to apps / dragging windows around if a keybind has been triggered | bool | `false` |
| scroll_event_delay | In ms, how many ms to wait after a scroll event to allow passing another one for the binds | int | `300` |
| window_direction_monitor_fallback | If enabled, moving a window or focus over the edge of a monitor with a direction will move it to the next monitor in that direction | bool | `true` |
| workspace_back_and_forth | If enabled, an attempt to switch to the currently focused workspace will instead switch to the previous workspace, akin to i3's `_auto_back_and_forth_` | bool | `false` |
| workspace_center_on | Whether switching workspaces should center the cursor on the workspace (0) or on the last active window for that workspace (1) | int | `1` |

### XWayland

_Subcategory `xwayland.`_ 

| name | description | type | default |
| --- | --- | --- | --- |
| enabled | Allow running applications using X11 | bool | `true` |
| create_abstract_socket | Create the [abstract Unix domain socket](../../40-extra/50-xwayland/#abstract-unix-domain-socket) for XWayland connections. XWayland restart is required for changes to take effect; Linux only | bool | `false` |
| force_zero_scaling | Forces a scale of 1 on xwayland windows on scaled displays | bool | `false` |
| use_nearest_neighbor | Uses the nearest neighbor filtering for xwayland apps, making them pixelated rather than blurry | bool | `true` |

### OpenGL

_Subcategory `opengl.`_ 

| name | description | type | default |
| --- | --- | --- | --- |
| nvidia_anti_flicker | Reduces flickering on nvidia at the cost of possible frame drops on lower-end GPUs. On non-nvidia, this is ignored | bool | `true` |

### Render

_Subcategory `render.`_ 

| name | description | type | default |
| --- | --- | --- | --- |
| cm_auto_hdr | Auto-switch to HDR in fullscreen when needed. `0` - disabled, `1` - switch to `hdr`, `2` - switch to `hdredid`. Options: [0 - 2] | int | `1` |
| cm_enabled | Whether the color management pipeline should be enabled or not. Requires restart | bool | `true` |
| cm_sdr_eotf | Default transfer function for displaying SDR apps. `default` - Use default value (sRGB), `gamma22` - treat unspecified as Gamma 2.2, `gamma22force` - treat unspecified and sRGB as Gamma 2.2, `srgb` - treat unspecified as sRGB. Options: `"default"`/`"gamma22"`/`"gamma22force"`/`"srgb"` | str | `"default"` |
| commit_timing_enabled | Enable commit timing proto. Requires restart | bool | `true` |
| ctm_animation | Whether to enable a fade animation for CTM changes (hyprsunset). 2 means "auto" which disables them on Nvidia | int | `2` |
| direct_scanout | Enables direct scanout. Direct scanout attempts to reduce lag when there is only one fullscreen application on a screen (game). It is also recommended to set this to false if the fullscreen application shows graphical glitches. `0` - disabled, `1` - enabled, `2` - auto (enabled with content type 'game'). Options: [0 - 2] | int | `0` |
| expand_undersized_textures | Whether to expand undersized textures along the edge, or rather stretch the entire texture | bool | `true` |
| fp16_sdr_tf | Internal workbuffer transfer function for fp16 in SDR mode. 0 - monitor, 1 - linear | int | `0` |
| icc_vcgt_enabled | Enable sending VCGT ramps to KMS with ICC profiles | bool | `true` |
| keep_unmodified_copy | Keep unmodified SDR frame copy for screensharing. `0` - disabled, `1` - enabled, `2` - auto (enabled in HDR with SDR modifiers). Set to 1 if screenshots are transparent. Options: [0 - 2] | int | `2` |
| new_render_scheduling | Automatically uses triple buffering when needed, improves FPS on underpowered devices | bool | `false` |
| not_shown_fifo_lock | Control fifo locking for not shown surfaces. always - use fifo lock for any surface, ignore_unfocused - ignore render_unfocused windows, never - skip locking invisible surfaces | int | `0` |
| non_shader_cm | Enable CM without shader. `0` - disable, `1` - whenever possible, `2` - DS and passthrough only, `3` - disable and ignore CM issues. Options: [0 - 3] | int | `2` |
| non_shader_cm_interop | `0` - external ctm (hypersunset, etc.) is disabled in fullscreen, `1` - external ctm is enabled in fullscreen, `2` - external ctm is disabled for fullscreen photo/video/game content types. Options: [0 - 2] | int | `2` |
| send_content_type | Report content type to allow monitor profile autoswitch (may result in a black screen during the switch) | bool | `true` |
| use_fp16 | Use FP16 buffers internally. `0` - disabled, `1` - enabled, `2` - enabled in hdr mode. Options: [0 - 2] | int | `2` |
| use_shader_blur_blend | Use experimental blurred bg blending (glitched on rotated screens). Set to `true` if blur is missing with fp16 or `keep_unmodified_copy` | bool | `false` |
| xp_mode | Disables back buffer and bottom layer rendering | bool | `false` |


`cm_auto_hdr` requires `--target-colorspace-hint-mode=source` mpv option to work with mpv versions greater than v0.40.0

### Cursor

_Subcategory `cursor.`_ 

| name | description | type | default |
| --- | --- | --- | --- |
| default_monitor | The name of a default monitor for the cursor to be set to on startup (see `hyprctl monitors` for names) | str | `[[Empty]]` |
| enable_hyprcursor | Whether to enable hyprcursor support | bool | `true` |
| hide_on_key_press | Hides the cursor when you press any key until the mouse is moved | bool | `false` |
| hide_on_tablet | Hides the cursor when the last input was a tablet input until a mouse input is done | bool | `false` |
| hide_on_touch | Hides the cursor when the last input was a touch input until a mouse input is done | bool | `true` |
| hotspot_padding | The padding, in logical px, between screen edges and the cursor | int | `0` |
| inactive_timeout | In seconds, after how many seconds of cursor's inactivity to hide it. Set to `0` for never | float | `0` |
| invisible | Don't render cursors | bool | `false` |
| min_refresh_rate | Minimum refresh rate for cursor movement when `no_break_fs_vrr` is active. Set to minimum supported refresh rate or higher | int | `24` |
| no_break_fs_vrr | Disables scheduling new frames on cursor movement for fullscreen apps with VRR enabled to avoid framerate spikes (may require `no_hardware_cursors` set to `1`). `0` - disabled, `1` - enabled, `2` - auto (enabled with content type 'game'). Options: [0 - 2] | int | `2` |
| no_hardware_cursors | Disables hardware cursors. `0` - use hw cursors if possible, `1` - don't use hw cursors, `2` - auto (disable when tearing). Options: [0 - 2] | int | `2` |
| no_warps | If true, will not warp the cursor in many cases (focusing, keybinds, etc) | bool | `false` |
| persistent_warps | When a window is refocused, the cursor returns to its last position relative to that window, rather than to the centre | bool | `false` |
| warp_back_after_non_mouse_input | Warp the cursor back to where it was after using a non-mouse input to move it, and then returning back to mouse | bool | `false` |
| warp_on_change_workspace | Move the cursor to the last focused window after changing the workspace. `0` - Disabled, `1` - Enabled, `2` - Force (ignores cursor:no_warps option). Options: [0 - 2] | int | `0` |
| warp_on_toggle_special | Move the cursor to the last focused window when toggling a special workspace. `0` - Disabled, `1` - Enabled, `2` - Force (ignores cursor:no_warps option). Options: [0 - 2] | int | `0` |
| sync_gsettings_theme | Sync xcursor theme with gsettings, it applies cursor-theme and cursor-size on theme load to gsettings making most CSD gtk based clients use same xcursor theme and size | bool | `true` |
| use_cpu_buffer | Makes HW cursors use a CPU buffer. Required on Nvidia to have HW cursors. `0` - disabled, `1` - enabled, `2` - auto (enabled with nvidia). Options: [0 - 2] | int | `2` |
| zoom_detached_camera | Detach the camera from the mouse when zoomed in, only ever moving the camera to keep the mouse in view when it goes past the screen edges | bool | `true` |
| zoom_disable_aa | Disable antialiasing when zooming, which means things will be pixelated instead of blurry | bool | `false` |
| zoom_factor | The factor to zoom by around the cursor. Like a magnifying glass. Minimum 1.0 (meaning no zoom) | float | `1.0` |
| zoom_rigid | Whether the zoom should follow the cursor rigidly (cursor is always centered if it can be) or loosely | bool | `false` |


### Ecosystem

_Subcategory `ecosystem.`_

| name | description                                                              | type | default |
| --- |--------------------------------------------------------------------------| --- | --- |
| no_update_news | Disable the popup that shows up when you update hyprland to a new version | bool | `false` |
| no_donation_nag | Disable the popup that shows up twice a year encouraging to donate | bool | `false` |
| enforce_permissions | Whether to enable [permission control](../100-advanced-configuration/40-permissions) | bool | `false` |

### Quirks

_Subcategory `quirks.`_

| name | description | type | default |
| --- | --- | --- | --- |
| prefer_hdr | Report HDR mode as preferred. `0` - disabled, `1` - always, `2` - gamescope only. Options: [0 - 2] | int | `0` |
| skip_non_kms_dmabuf_formats | do not report dmabuf formats which cannot be imported into KMS | bool | `false` |

Some clients expect monitor to be in HDR mode prior to the client start. This breaks auto HDR activation and can cause whitescreen and flickering. Use `prefer_hdr` to fix it.

### Input Capture
_Subcategory `input-capture.`_
| name | description | type | default |
| --- | --- | --- | --- |
| capture_modifiers | if enabled, modifiers are also captured and sent to the program | bool | `false` |
| enforce_barriers | if enabled, throw a wayland error when an invalid barrier is received | bool | `true` |


### Debug

> [!WARNING]
> Only for developing and testing

_Subcategory `debug.`_

| name | description | type | default |
| --- | --- | --- | --- |
| colored_stdout_logs | Enables colors in the stdout logs. | bool | `true` |
| damage_blink | (epilepsy warning!) Flash areas updated with damage tracking | bool | `false` |
| damage_tracking | Redraw only the needed bits of the display. Do **not** change. (default: full - 2) monitor - 1, none - 0 | int | `2` |
| disable_logs | Disable logging to a file | bool | `true` |
| disable_scale_checks | Disables verification of the scale factors. Will result in pixel alignment and rounding errors. | bool | `false` |
| disable_time | Disables time logging | bool | `true` |
| ds_handle_same_buffer | special case for direct scanout with unmodified buffer | bool | `true` |
| ds_handle_same_buffer_fifo | special case for direct scanout with unmodified buffer unlocks fifo | bool | `true` |
| enable_stdout_logs | Enables logging to stdout | bool | `false` |
| error_limit | Limits the number of displayed config file parsing errors | int | `5` |
| error_position | Sets the position of the error bar. `0` - top, `1` bottom. Options: [0 - 1] | int | `0` |
| fifo_pending_workaround | fifo workaround for empty pending list | bool | `false` |
| full_cm_proto | Claims support for all cm proto features (requires restart) | bool | `false` |
| gl_debugging | Enables OpenGL debugging with glGetError and EGL_KHR_debug, requires a restart after changing | bool | `false` |
| invalidate_fp16 | Allow fp16 buffer invalidation (invalidation increases performance but produces glitches on some systems). 0 - not allowed, 1 - allowed, 2 - not allowed on nvidia | int | `2` |
| manual_crash | Set to 1 and then back to 0 to crash Hyprland | int | `0` |
| overlay | Print the debug performance overlay. Disable VFR for accurate results | bool | `false` |
| pass | Enables render pass debugging | bool | `false` |
| render_solitary_wo_damage | render solitary window with empty damage | bool | `false` |
| suppress_errors | If enabled, Hyprland will not display config file parsing errors | bool | `false` |
| vfr | Controls the VFR status of Hyprland. Heavily recommended to leave enabled to conserve resources | bool | `true` |
| log_damage | Enables logging the damage | bool | `false` |

### Experimental
_Subcategory `experimental.`_
| name | description | type | default |
| --- | --- | --- | --- |
| wp_cm_1_2 | allow wp-cm-v1 version 2 | bool | `false` |

### More

There are more config options described in other pages, which are layout- or
circumstance-specific. See the sidebar for more pages.

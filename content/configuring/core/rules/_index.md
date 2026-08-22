---
weight: 60
title: Rules
---

Rules can be used to enforce specific options for matched windows/workspaces/layers.

Rules can be named and anonymous.

Rules are split into two categories of parameters: _props_ and _effects_.
Props are the fields inside the `match` table, which are used to determine if a window should get the rule.
Effects are what is applied.

Rules are evaluated top to bottom, but all named rules are evaluated first, then all anonymous ones.

_All_ props must match for a rule to be applied.
You can have as many props and effects per rule as you want, and in any order, as long as:
- Match field must have at least one prop specified
- Props are not repeated

In the output of the `hyprctl clients` command, `fullscreen` refers to `fullscreen_state_internal`, and `fullscreenClient` refers to `fullscreen_state_client`.

Read more about rules in the following pages:
{{< subpage_list >}}

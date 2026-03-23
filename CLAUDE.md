# Simply Love Theme — Developer Notes

## How UI elements are structured

This is a StepMania 5 theme. UI elements on each screen come from two layers:

1. **Engine-level actors** — controlled via `metrics.ini` (header text, menu timer, style icon, footer bar)
2. **Lua decorations** — loaded from `BGAnimations/` and `Graphics/` as ActorFrames

---

## Hiding UI elements on a screen

### The grey header/footer bars (Lua quads)

Defined in `Graphics/_header.lua` and `Graphics/_footer.lua`. These are Lua-drawn quads, not engine elements.

- To hide the **footer bar** globally: add `:visible(false)` to its `InitCommand` in `_footer.lua`.
- To hide the **header bar** on specific screens: add screen names to the `hiddenScreens` table inside the `ScreenChangedMessageCommand` of the Quad in `_header.lua`.

### The header text ("EVALUATION", "Select Music", etc.)

The text comes from `Languages/en.ini` (`HeaderText=...` per screen section) and is rendered by `Graphics/_header.lua` via `LoadFont("Common Header")`. It is part of the Header decoration loaded by `BGAnimations/ScreenWithMenuElements decorations.lua`.

To hide it on a screen, add to that screen's section in `metrics.ini`:
```ini
HeaderOnCommand=visible,false
```

### The menu timer

Controlled entirely via `metrics.ini`. To hide it:
```ini
TimerOnCommand=visible,false
```

### The style icon (game type + pad layout image)

Engine-drawn. To hide it:
```ini
StyleIcon=false
```

### Footer bar visibility per screen

```ini
FooterOnCommand=visible,false
```

---

## Example: fully hiding the top menu on ScreenEvaluation

```ini
[ScreenEvaluation]
ShowHeader=false          ; hides engine's built-in header (belt-and-suspenders)
HeaderOnCommand=visible,false  ; hides the Lua header decoration (text + bar)
TimerOnCommand=visible,false   ; hides the menu timer
StyleIcon=false                ; hides the ITG logo + pad icon
FooterOnCommand=visible,false  ; hides the footer bar
```

Screens that `Fallback="ScreenEvaluation"` (e.g. `ScreenEvaluationStage`, `ScreenEvaluationNonstop`, `ScreenEvaluationSummary`) inherit these settings automatically.

---

## Key files

| File | Purpose |
|---|---|
| `metrics.ini` | All screen metrics: layout, timers, visibility commands |
| `Graphics/_header.lua` | Lua-drawn header bar + header text |
| `Graphics/_footer.lua` | Lua-drawn footer bar |
| `BGAnimations/ScreenWithMenuElements decorations.lua` | Loads header and footer decorations for all menu screens |
| `BGAnimations/ScreenEvaluation decorations.lua` | Evaluation-specific decorations (extends ScreenWithMenuElements) |
| `Languages/en.ini` | String table — `HeaderText=` sets the text shown in the header |

---
name: winforms-hero-microcopy
description: Use when a Windows Forms WinForms screen feels empty, sterile, or bare and the user wants to add a Value Proposition (hero copy - headline + supporting sentence) and/or microcopy (helper labels, captions, field hints, empty-state text, footer taglines). Visual-only additions in FormName.Designer.cs; dynamic microcopy adds presentation-only code in FormName.cs that only sets Label.Text on events that already exist. Zero new functionality, no new inputs or behavior, no external libraries. Follows the Segoe UI / Office-look guidelines and must pair with any palette chosen via winforms-ui-redesign.
---

# WinForms Hero Copy & Microcopy (Kill the Bare White Space)

Fill sparse Windows Forms screens with **written words, not chrome**: a value
proposition (hero) for the main window, helper text near inputs, captions in
empty lists, and a small footer tagline. Purely visual - every added control is
a read-only `Label` or `LinkLabel` with `Enabled` behavior untouched.

This skill **additively restyles with copy**, it does not add features.

## Hard rules

1. **No functionality.** New controls are `Label` (or at most `LinkLabel` whose
   `LinkClicked` is only wired **if** the user explicitly asks and supplies the
   destination). No `TextBox`, no `Button`, no timers, no logic.
2. **All static copy goes in `FormName.Designer.cs`.** New labels follow the exact
   designer pattern used in that file (new + commented block + property block +
   `Controls.Add`). Keep designer-file-compatible: literal values only.
3. **Dynamic microcopy lives in the form's `.cs` file** and is *presentation-only*:
   it may only assign `.Text` (and at most `ForeColor`) to labels inside event
   handlers or lifecycle points that **already exist** (`Load`, existing
   `TextChanged`/`SelectedIndexChanged` handlers). It must not:
   - add new event subscriptions,
   - validate, early-return, block, or alter program flow,
   - call dialogs or change `Enabled` beyond purely visual `Label.ForeColor`.
   The one exception: a private `void UpdateEmptyStateLabel()`-style helper is
   fine **only if** it is called from handlers that already exist.
4. **No external dependencies, images, emoji, or icon fonts.** Text only.
5. **Never edit any other form than the ones in scope.** If the project has a
   palette chosen via `winforms-ui-redesign`, reuse it. If not, default to the
   neutral scheme (gray-95 secondary text, Word Blue accent) and say so.

## What goes where (placement map)

| Sparse area | Filler | Control |
|---|---|---|
| Top of a main window | Hero: headline (16-14 F Segoe UI Bold, accent or gray-160) + one supporting sentence (9 F, gray-95) | Two `Label`s, `AutoSize`, top-left, inside the existing top panel or docked `Panel` header |
| Left column / "dead" vertical space | Value bullets: 2-3 short lines, each `Label`, 8 F gray-95, one actionable verb each | A `FlowLayoutPanel` docked side, `FlowDirection.TopDown`, no borders |
| Under an input | Helper text ("We'll only email you a receipt.") | One `Label`, 8 F gray-128, directly below, sharing the input's left edge |
| Empty `ListBox`/`DataGridView`/`Panel` | Empty-state copy ("No entries yet - add one to get started.") | One `Label`, `Dock = Fill`, `TextAlign = MiddleCenter`, gray-128, 9 F |
| Bottom of a fixed dialog | Reassurance / version line ("Your changes stay on this PC.") | One small 8 F gray-128 `Label`, bottom-left or centered, clear of buttons |
| Any long silence (long task, disabled period) | Not this skill's job - flag it as functional, don't fake it with a label |

## Copywriting rules (plain, friendly, Office-clean)

- **Sentence case everywhere.** No ALL CAPS, no Title Case headlines.
- **Headline: 3-7 words**, states the value, no trailing period.
  Good: `Manage your day without leaving this window.`
- **Subline: one sentence**, 8-14 words, tells *what you can do here*, not
  marketing adjectives. Avoid "revolutionary", "best", "seamless", "let's".
- **Helper text:** one short present-tense sentence, no trailing period needed,
  starts with what the user does ("Type the account number…") or what the app does
  ("Only you see this list.").
- **Empty states:** name the empty thing, then the single cheapest next action.
  "No items yet — Add one to get started." Match any real action button text.
- **Footer tagline:** 6-12 words, no contact details unless the user provides them.
- Never invent facts: no URLs, support phone numbers, storage guarantees, pricing,
  or claims about privacy/security. Ask the user if the copy implies a fact they
  haven't given (e.g. "your data stays on this PC") — or use neutral wording.
- Plural/singular must match reality of the data behind the control.
- No emoji, no ellipsis decoration, no exclamation marks.

## Dynamic microcopy: allowed patterns

1. **Empty-state swap** (`SelectedIndexChanged`, `TextChanged`, `Load`):
   `emptyStateLabel.Text = items.Count == 0 ? "No items yet — Add one." : "";`
   (assign `""`, never `Visible = false` unless the label was there before).
2. **Hint preview** in an existing `TextBox` handler:
   `hintLabel.Text = string.IsNullOrWhiteSpace(nameTextBox.Text) ? "Start typing a name." : "";`
3. **Counter-style caption** in an existing refresh/load handler:
   `itemsCaptionLabel.Text = $"{itemsList.Items.Count} items";`
Every dynamic label must have a sensible **default value in the Designer** so the
screen is never blank if the handler never fires. Count each new line of logic:
if a pattern needs more than ~3 lines in `.cs`, stop and choose static copy.

## Workflow

1. **Inventory** the target form(s): read `Designer.cs` and the form's `.cs`.
   Map: existing white-space zones, existing events the `.cs` already subscribes
   to, current font/palette (or the one chosen via `winforms-ui-redesign`).
2. **Propose, don't dump.** Plan at most **one hero + up to 3 fillers** per form.
   List each planned copy string for the user.
3. **Ask the user** (one batch, multi-select where appropriate):
   - Which areas to fill (top hero / side bullets / input helpers / empty states /
     footer)? Default all that apply with rationale.
   - Tone: *Friendly professional (Recommended)* · Warm conversational · Strictly
     neutral business.
   - Hero content: let the user supply product name + one-line purpose, or offer
     to draft 2-3 candidate headline/subline pairs to pick from.
   - Dynamic empty-state swap: yes (only if an existing event already fires on
     that list/input) or static-only.
4. **Write the copy** per the rules above. Read existing button/field labels first
   and reuse their exact verbs and nouns ("Add entry", not "new record").
5. **Edit `Designer.cs`**: add labels following the file's existing block pattern;
   set `Font` per the chosen size scale (hero 16F/14F, subline and helper 8-9F),
   `ForeColor` per palette, positions on the design grid (12 px padding,
   32 px row pitch, aligned left edges).
6. **Edit `.cs`** only for the user-approved dynamic pattern, presentation-only,
   inside existing handlers; add an `emptyStateLabel`-style helper method if the
   same swap is needed in more than one existing handler.
7. **Verify**: `dotnet build` zero new errors. Then re-read the final `Designer.cs`
   to confirm no added control usurped `TabIndex` of an input (new labels go last
   in `Controls.Add` order, TabIndex at the end of the list, no `TabStop = true`
   needed for labels).
8. **Report**: each string added, where it lands, which file each change is in,
   and any claim that needs the user's confirmation (privacy, version, support).

## Do / Don't

**Do**
- Match the existing project's terminology and button capitalization.
- Keep copy shorter than the space it fills - text should not push layout wider.
- Prefer two well-placed sentences over four mediocre ones.
- Keep `AutoSize = true` on copy labels so font scaling doesn't clip.

**Don't**
- Don't add marketing superlatives, countdowns, exclamation marks, or emoji.
- Don't replace existing `Text` values of buttons/inputs - only add labels.
- Don't wire new events, timers, tooltips, or any logic that changes behavior.
- Don't touch `Program.cs`, `.csproj`, or `.resx`.
- Don't fake dynamic status ("Loading…", "Syncing…") - flags it as functional.
- Don't restyle existing controls (that's `winforms-ui-redesign`'s job); only
  add copy-related controls and, if explicitly asked, coordinate with that skill.

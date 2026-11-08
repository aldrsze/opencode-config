---
name: winforms-ui-redesign
description: Use when the user wants to redesign, restyle, or improve the visual design of an EXISTING Windows Forms project - fonts, colors, backgrounds, spacing, proportionality, alignment. Edits FormName.Designer.cs (all visual code) and FormName.cs only if visual styling lives there. Zero functional changes, no new controls beyond decorative layout panels, no external libraries. Produces a friendly, professional Microsoft Office desktop-app look. Always ends by asking the user about fonts, palette, and style intensity with recommended choices.
---

# WinForms UI Redesign (Office-Style, Existing Project)

Improve the visual design of an **existing** Windows Forms project: fonts, colors,
backgrounds, proportions, spacing, and alignment. Target look: a clean, friendly
**Microsoft Office desktop app** - light, neutral, generous whitespace, one accent
color, consistent typography.

This skill **restyles, it does not rebuild**. Behavior, functionality, event wiring,
and control names must survive untouched.

## Hard rules

1. **No functional changes.** Never alter event subscriptions, `DialogResult`,
   `AcceptButton`/`CancelButton`, validation, logic, or control *behavior* properties.
2. **Keep all control field names unchanged** (`nameTextBox` stays `nameTextBox`).
   Renaming breaks event handlers in the `.cs` file.
3. **All visual edits go in `FormName.Designer.cs`.** Touch the form's `.cs` file
   only when it contains visual properties that fight the redesign (e.g. a runtime
   `this.ForeColor = Color.Red;` in code) or when code-built controls need styling.
   Report every change made to a `.cs` file.
4. **No new functionality.** Adding a decorative element (header panel, separator)
   is allowed; adding inputs, buttons, or behavior is not.
5. **No external dependencies.** Only `System.Drawing` and `System.Windows.Forms`.
   No NuGet, no custom `OnPaint`, no icon/resource files, no `.resx` edits.
6. **Designer-file-compatible code only.** Inside `InitializeComponent()` every
   value must be a literal or `new` expression - no helper methods, constants,
   fields, or `Color.FromArgb` cached in variables. The file must still load in
   the Visual Studio designer.
7. **Rebuildable:** after edits, `dotnet build` (or the project's normal build)
   must succeed with zero new errors.

## Office look, concretely

- **Typography:** one family app-wide - **Segoe UI** (font size chosen with the
  user, default 9F). Headers may use Segoe UI Bold at 12-14F. Window titles inherit
  the system font; do not set the form `Font` so the title bar matches Windows.
  Alternative families are offered in the questions step (Calibri, Verdana, Tahoma).
- **Backgrounds:** white (`Color.White`) or very light gray surfaces
  (`Color.FromArgb(249, 249, 249)`); page-fill content on white; panels/chrome in
  the chosen mild gray. No dark gradients, no saturated background fills.
- **One accent color** from a standard Office palette (chosen with the user), used
  for: primary action button, header/title strip, selected states, DataGridView
  header background. Everything else stays gray/neutral.
- **Secondary text** (captions, hints, status labels): gray
  `(Color.FromArgb(95, 95, 95))` instead of pure black - via
  `ForeColor` only, never via disabled state.
- **Proportions and grid:**
  - Form padding **12 px**; gaps **8 px** between related controls,
    **16-24 px** between groups.
  - One consistent row pitch (typically **32 px** from the form's font baseline).
  - Align all field labels on a shared left edge; right-align label text.
  - Buttons: standard **height 30-32 px**; primary button same size as others,
    distinguished only by accent `BackColor`/white text. Bottom-right on dialogs,
    bottom or top-right on main windows.
  - Inputs (`TextBox`, `ComboBox`) match each other's width by column, `$`-style
    proportional to the label column.
  - `AutoScaleMode.Font` and `AutoDimensions`: keep the existing
    `AutoScaleDimensions`/`AutoScaleMode` values as-is unless the font size changes.
- **Containers:** `GroupBox` or `Panel` with `Padding` set for visual grouping;
  `Dock` for top chrome, main grids/lists, bottom status and button strips;
  `Anchor` for resize behavior (never resize-anchor dialogs).
- **DataGridView / ListView styling** (when present):
  headers: accent or gray background (`Color.FromArgb(243, 243, 243)`), no
  `EnableHeadersVisualStyles=false` gradients; `AlternatingRowsDefaultCellStyle`
  light gray `Color.FromArgb(249, 249, 249)`; `BorderStyle = None` on the grid
  with a bordered host panel; `RowHeadersVisible` false when unused.
- **Buttons:** keep `UseVisualStyleBackColor = true` on neutral buttons; set
  `FlatStyle = Flat` + `FlatAppearance.BorderColor` only when the user opts for a
  "flat" look. Do not restyle buttons on MessageBox-like small dialogs.
- **Do not** introduce gradients, rotation, animations, images, emoji, icons, or
  custom-drawn controls.

## Workflow

1. **Inventory.** Locate the WinForms project: `.csproj`, `Program.cs`,
   `FormName.Designer.cs` / `FormName.cs` pairs. Read the `Designer.cs` files and
   note the current fonts, colors, sizes, spacing, and alignment problems
   (inconsistent fonts, mixed row heights, unaligned edges, oversized/undersized
   controls, clashing colors, missing anchors).
2. **Check the `.cs` files** for visual properties set in code (`Font`, `BackColor`,
   `ForeColor`, `Location`, `Size`, `Text` on labels, etc.) - these override the
   Designer and must be flagged or replaced.
3. **Ask the user** (see "Questions" below) about font family, base size, palette,
   style intensity, grid styling, and window behavior. Recommend one option per
   question and explain **why**.
4. **Plan the layout math** before editing: compute a shared left edge, row pitch,
   column grid, and button strip placement per form from the existing control list.
5. **Edit `Designer.cs`:** restructure `InitializeComponent()` block by block.
   Keep each control's commented block; update only visual properties; add
   decorative containers (`Panel`, separator) only where layout needs them.
   Set form-level padding, background, and font last, before `Controls.Add` calls
   and `ResumeLayout`.
6. **Edit `.cs` only if flagged** in step 2 - move that styling to the Designer or
   update the code-built styling to the chosen palette. Say exactly what changed.
7. **Verify:** build the project. If it fails, fix only the edit-related breakage.
8. **Report** per form: what was changed (font, palette, sizes, spacing, alignment),
   files touched, anything the user should eyeball at runtime (e.g. long labels).

## Questions to ask (always ask; recommend the first option)

Use the question tool, all questions in one batch, with rationale in each option.

1. **Font family**
   - *Segoe UI (Recommended)* - Windows native, clean, best legibility at small
     sizes; the Office/modern-Windows look.
   - Calibri - softer, warmer; the classic Office 2007-2019 document feel.
   - Verdana / Tahoma - wider spacing, easier reading on low-DPI screens.
2. **Base font size**
   - 9 pt (Recommended) - standard for business apps; fits dense data grids.
   - 10 pt - friendlier for touch/unfamiliar users; needs ~10% larger forms.
   - 11 pt - large, accessible; only if the user asks for oversized text.
3. **Color palette** (accent + neutral scheme)
   - *Word Blue `#2B579A` (Recommended)* - confident, corporate, excellent white
     contrast; reads as "Office" instantly.
   - Excel Green `#217346` - calm, fresh; suits inventory/finance/data apps.
   - Neutral Gray + dark-blue hover - maximum sobriety; Office "Fluent light".
   - Teal `#0F6CBD` (Outlook-ish) - modern, friendly, slightly techy.
4. **Style intensity**
   - *Accent header strip + neutral body (Recommended)* - a colored top panel with
     the form title, white content area; distinctive but clean.
   - Fully neutral - system borders and buttons, only fonts/spacing/color tuned;
     most conservative change.
   - Full flat restyle - flat buttons with accent primary, gray dividers, styled
     grids; the most dramatic change, best for dense data-heavy windows.
5. **Data grid styling** (if `DataGridView`/`ListView` exist)
   - Yes, style headers + alternating rows (Recommended) - biggest visual win.
   - Minimal - keep default grid chrome, only adjust fonts/size.
6. **Window behavior**
   - Keep current form borders/sizing (Recommended) - zero behavioral impact.
   - Make main windows resizable with anchored/pinned content; keep dialogs fixed.
7. **Scope**
   - Redesign all forms consistently (Recommended).
   - Only specific forms (which?).

If the user answers with a free-text choice not listed, honor it and fold it into
the palette/spacing plan. If the user skips questions, apply the *Recommended*
options silently and say so in the report.

## Do / Don't

**Do**
- Read before writing: inventory first, edit second.
- Preserve `SuspendLayout` / `ResumeLayout` / `PerformLayout` structure and the
  `components`/`Dispose` pattern.
- Match sizing in `$`-proportions: label column ~ 1/3, input column ~ 2/3 width.
- Keep `TabIndex` order unchanged; fix only stray values that break tab flow.
- Apply the **same font, spacing, and palette math across every form** in scope.
- After editing, do a quick mental compile: every referenced field still declared,
  every `+=` handler untouched.

**Don't**
- Don't rename controls, events, or handles clauses.
- Don't add images, icons, gradients, colors not in the chosen palette, or extra
  fonts (one family + bold weight only).
- Don't touch `Program.cs`, `.csproj`, or `.resx`.
- Don't add controls that need new event handlers or logic.
- Don't set the form `Font` if that changes the title-bar/system metrics the user
  relies on - prefer per-control fonts when in doubt.
- Don't change `ClientSize` unless the new layout genuinely needs room; prefer
  re-flowing existing space and mention the resize in the report if you do.

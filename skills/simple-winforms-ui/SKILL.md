---
name: simple-winforms-ui
description: Use when the user wants a Windows Forms (WinForms) UI written in plain C#. Puts all UI code in the Form's .Designer.cs file and logic in the Form's .cs file, using only built-in System.Windows.Forms and System.Drawing, with no NuGet packages, third-party libraries, or extensions.
---

# Simple WinForms UI (Pure C#, Designer.cs)

Build Windows Forms user interfaces in C# using only what ships with .NET. **All UI/control code goes in `FormName.Designer.cs`.** Keep every design simple.

## Hard rules

1. **No external dependencies.** Only these namespaces are allowed:
   - `System`
   - `System.ComponentModel` (only for `IContainer` in the Designer)
   - `System.Drawing`
   - `System.Windows.Forms`
   - `System.Collections.Generic` / `System.Linq` / `System.IO` only when the logic truly needs them
2. **No NuGet packages, no UI libraries** (no MetroFramework, Guna, DevExpress, Telerik, MaterialSkin, etc.), and no custom-drawn controls or `OnPaint` overrides unless the user asks.
3. **All UI code lives in `FormName.Designer.cs`**: control declarations, creation, properties, layout and event wiring. The form's `.cs` file holds only the constructor and event handler logic.
4. **Do not produce `.resx` files** unless the user asks.
5. **Keep it simple.** If a design choice adds complexity, pick the simpler option.

## File structure

Each form is a `partial class` split across two files:

| File | Contains |
|---|---|
| `MainForm.Designer.cs` | `Dispose`, `InitializeComponent()`, control field declarations, all properties, layout, event subscriptions (`+=`) |
| `MainForm.cs` | Constructor calling `InitializeComponent()`, event handler methods, validation and logic |
| `Program.cs` | `Main` marked `[STAThread]` |
| `*.csproj` | Project file, no `PackageReference` |

## Designer.cs conventions

- Follow the standard Visual Studio designer pattern so the file stays compatible with the designer.
- Declare controls as `private System.Windows.Forms.X name;` at the bottom of the file.
- Inside `InitializeComponent()`:
  1. Create every control (`this.saveButton = new System.Windows.Forms.Button();`).
  2. Call `SuspendLayout()` on the form and on containers.
  3. Configure each control in its own commented block (`// saveButton`): `Name`, `Text`, `Location`, `Size`, `TabIndex`, `Dock`/`Anchor`, `UseVisualStyleBackColor = true`.
  4. Subscribe events: `this.saveButton.Click += new System.EventHandler(this.saveButton_Click);`
  5. Configure the form (`// MainForm`), add controls with `Controls.Add`, then `ResumeLayout(false)` / `PerformLayout()`.
- Mark the generated file with `#nullable disable` if the project enables nullable.
- Use clear control names (`saveButton`, `nameTextBox`), never `button1`.
- The `.cs` file must not create or position controls.

## Design principles

- One form, one purpose. Avoid multi-window flows unless requested.
- Use the **default system look**: default fonts, default control styles, `SystemColors`. Do not restyle every control.
- At most **one accent color**, and only if the user wants color. Otherwise stay with system defaults.
- Consistent spacing: **12 px** form padding, **8 px** between related controls, **16 px** between groups.
- Consistent sizes: buttons about `90 x 30`, text boxes about `200-250` wide, labels auto-sized.
- Short, plain labels ("Name:", "Save", "Cancel"). No decorative text or emoji.
- Use at most 8-10 controls on a screen unless the task demands more.

## Layout approach

Designer files normally use explicit `Location` and `Size`. Keep that, since it is the simplest approach and matches the designer, but make it tidy:

- Align controls on a shared left edge and use consistent vertical steps (for example 32 px per row).
- Use `Anchor` so inputs and buttons behave when the window is resized.
- Use `Dock` for main areas (`DataGridView`, `ListBox`, `MenuStrip`, `StatusStrip`).
- Use `TableLayoutPanel` or `FlowLayoutPanel` only when a form has many rows or a button bar that clearly benefits.

## Form defaults (set in Designer.cs)

```csharp
this.AutoScaleDimensions = new System.Drawing.SizeF(7F, 15F);
this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
this.ClientSize = new System.Drawing.Size(400, 250);
this.FormBorderStyle = System.Windows.Forms.FormBorderStyle.FixedDialog;
this.MaximizeBox = false;
this.StartPosition = System.Windows.Forms.FormStartPosition.CenterScreen;
this.Text = "Window Title";
```

Also set `AcceptButton` / `CancelButton` where Enter/Esc make sense.

## Template

### MainForm.Designer.cs

```csharp
#nullable disable
namespace SimpleApp
{
    partial class MainForm
    {
        private System.ComponentModel.IContainer components = null;

        protected override void Dispose(bool disposing)
        {
            if (disposing && (components != null))
            {
                components.Dispose();
            }
            base.Dispose(disposing);
        }

        private void InitializeComponent()
        {
            this.nameLabel = new System.Windows.Forms.Label();
            this.nameTextBox = new System.Windows.Forms.TextBox();
            this.emailLabel = new System.Windows.Forms.Label();
            this.emailTextBox = new System.Windows.Forms.TextBox();
            this.saveButton = new System.Windows.Forms.Button();
            this.cancelButton = new System.Windows.Forms.Button();
            this.SuspendLayout();
            //
            // nameLabel
            //
            this.nameLabel.AutoSize = true;
            this.nameLabel.Location = new System.Drawing.Point(12, 20);
            this.nameLabel.Name = "nameLabel";
            this.nameLabel.TabIndex = 0;
            this.nameLabel.Text = "Name:";
            //
            // nameTextBox
            //
            this.nameTextBox.Anchor = System.Windows.Forms.AnchorStyles.Top
                | System.Windows.Forms.AnchorStyles.Left
                | System.Windows.Forms.AnchorStyles.Right;
            this.nameTextBox.Location = new System.Drawing.Point(80, 17);
            this.nameTextBox.Name = "nameTextBox";
            this.nameTextBox.Size = new System.Drawing.Size(250, 23);
            this.nameTextBox.TabIndex = 1;
            //
            // emailLabel
            //
            this.emailLabel.AutoSize = true;
            this.emailLabel.Location = new System.Drawing.Point(12, 52);
            this.emailLabel.Name = "emailLabel";
            this.emailLabel.TabIndex = 2;
            this.emailLabel.Text = "Email:";
            //
            // emailTextBox
            //
            this.emailTextBox.Anchor = System.Windows.Forms.AnchorStyles.Top
                | System.Windows.Forms.AnchorStyles.Left
                | System.Windows.Forms.AnchorStyles.Right;
            this.emailTextBox.Location = new System.Drawing.Point(80, 49);
            this.emailTextBox.Name = "emailTextBox";
            this.emailTextBox.Size = new System.Drawing.Size(250, 23);
            this.emailTextBox.TabIndex = 3;
            //
            // saveButton
            //
            this.saveButton.Anchor = System.Windows.Forms.AnchorStyles.Bottom
                | System.Windows.Forms.AnchorStyles.Right;
            this.saveButton.Location = new System.Drawing.Point(150, 140);
            this.saveButton.Name = "saveButton";
            this.saveButton.Size = new System.Drawing.Size(90, 30);
            this.saveButton.TabIndex = 4;
            this.saveButton.Text = "Save";
            this.saveButton.UseVisualStyleBackColor = true;
            this.saveButton.Click += new System.EventHandler(this.saveButton_Click);
            //
            // cancelButton
            //
            this.cancelButton.Anchor = System.Windows.Forms.AnchorStyles.Bottom
                | System.Windows.Forms.AnchorStyles.Right;
            this.cancelButton.DialogResult = System.Windows.Forms.DialogResult.Cancel;
            this.cancelButton.Location = new System.Drawing.Point(246, 140);
            this.cancelButton.Name = "cancelButton";
            this.cancelButton.Size = new System.Drawing.Size(90, 30);
            this.cancelButton.TabIndex = 5;
            this.cancelButton.Text = "Cancel";
            this.cancelButton.UseVisualStyleBackColor = true;
            this.cancelButton.Click += new System.EventHandler(this.cancelButton_Click);
            //
            // MainForm
            //
            this.AcceptButton = this.saveButton;
            this.AutoScaleDimensions = new System.Drawing.SizeF(7F, 15F);
            this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
            this.CancelButton = this.cancelButton;
            this.ClientSize = new System.Drawing.Size(348, 182);
            this.Controls.Add(this.nameLabel);
            this.Controls.Add(this.nameTextBox);
            this.Controls.Add(this.emailLabel);
            this.Controls.Add(this.emailTextBox);
            this.Controls.Add(this.saveButton);
            this.Controls.Add(this.cancelButton);
            this.FormBorderStyle = System.Windows.Forms.FormBorderStyle.FixedDialog;
            this.MaximizeBox = false;
            this.Name = "MainForm";
            this.StartPosition = System.Windows.Forms.FormStartPosition.CenterScreen;
            this.Text = "User Details";
            this.ResumeLayout(false);
            this.PerformLayout();
        }

        private System.Windows.Forms.Label nameLabel;
        private System.Windows.Forms.TextBox nameTextBox;
        private System.Windows.Forms.Label emailLabel;
        private System.Windows.Forms.TextBox emailTextBox;
        private System.Windows.Forms.Button saveButton;
        private System.Windows.Forms.Button cancelButton;
    }
}
```

### MainForm.cs

```csharp
using System;
using System.Windows.Forms;

namespace SimpleApp
{
    public partial class MainForm : Form
    {
        public MainForm()
        {
            InitializeComponent();
        }

        private void saveButton_Click(object sender, EventArgs e)
        {
            if (string.IsNullOrWhiteSpace(nameTextBox.Text))
            {
                MessageBox.Show("Please enter a name.", "Missing info",
                    MessageBoxButtons.OK, MessageBoxIcon.Warning);
                return;
            }

            MessageBox.Show($"Saved {nameTextBox.Text}.", "Success",
                MessageBoxButtons.OK, MessageBoxIcon.Information);
        }

        private void cancelButton_Click(object sender, EventArgs e)
        {
            Close();
        }
    }
}
```

### Program.cs

```csharp
using System;
using System.Windows.Forms;

namespace SimpleApp
{
    internal static class Program
    {
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}
```

### Project file

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net8.0-windows</TargetFramework>
    <UseWindowsForms>true</UseWindowsForms>
    <Nullable>disable</Nullable>
  </PropertyGroup>
</Project>
```

Target modern .NET (`net8.0-windows`) unless the user says .NET Framework. No `<PackageReference>` entries are allowed.

## Common controls (all built in)

`Label`, `TextBox`, `Button`, `CheckBox`, `RadioButton`, `ComboBox`, `ListBox`, `NumericUpDown`, `DateTimePicker`, `DataGridView`, `ListView`, `TabControl`, `MenuStrip`, `StatusStrip`, `GroupBox`, `Panel`, `MessageBox`, `OpenFileDialog`, `SaveFileDialog`.

## Do / Don't

**Do**
- Keep every control, property and layout setting in the Designer.cs file.
- Validate input before acting and show a short `MessageBox` on errors.
- Set `TabIndex` in reading order.
- Keep event handlers short in the `.cs` file; move logic into helper methods.

**Don't**
- Don't build or position controls in the `.cs` file (the only exception is data-driven content, such as filling a `ComboBox` or grid at runtime).
- Don't add themes, gradients, custom fonts, animations, or icons from outside the framework.
- Don't use third-party or NuGet packages, or reference extra assemblies.
- Don't add features the user didn't ask for.

## Workflow

1. Identify the form's single purpose and list the controls it needs.
2. Plan a simple grid: left edge at 12 px, one row every 32 px, buttons bottom right.
3. Write `FormName.Designer.cs` with all controls, properties, layout and event wiring.
4. Write `FormName.cs` with the constructor and event handlers only.
5. Add `Program.cs` and the `.csproj`.
6. Re-check against the hard rules: no libraries, all UI in Designer.cs, simple design.
7. Present the files and a one-line note on how to run it (`dotnet run`).
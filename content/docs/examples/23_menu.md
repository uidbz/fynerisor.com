---
title: Menu
type: docs
weight: 23
---

Demonstrates Menu and PopUpMenu widgets - context menus and menu systems.

![Menu Screenshot](/images/examples/23-menu.png)

## Features

- **MenuItem Creation**: fyne.NewMenuItem(label, callback)
- **Menu Separators**: Visual grouping with fyne.NewMenuItemSeparator()
- **PopUpMenu**: Display menu in overlay
- **Item States**: Disabled and Checked properties
- **Dynamic Updates**: Toggle item states at runtime

## Code

```js
require(["v0.3", "@gui"])

let statusLabel = widget.NewLabel("Click button to show context menu")

// Create menu items
let copyItem = fyne.NewMenuItem("Copy", () => {
	statusLabel.SetText("Copy selected")
})

let pasteItem = fyne.NewMenuItem("Paste", () => {
	statusLabel.SetText("Paste selected")
})

let cutItem = fyne.NewMenuItem("Cut", () => {
	statusLabel.SetText("Cut selected")
})
cutItem.Disabled = false

// Separator
let sep = fyne.NewMenuItemSeparator()

// Submenu items
let selectAllItem = fyne.NewMenuItem("Select All", () => {
	statusLabel.SetText("Select All selected")
})

let findItem = fyne.NewMenuItem("Find...", () => {
	statusLabel.SetText("Find selected")
})
findItem.Checked = false

// Create context menu
let editMenu = fyne.NewMenu("Edit", copyItem, cutItem, pasteItem, sep, selectAllItem, findItem)

// Create popup menu
let popupMenu = widget.NewPopUpMenu(editMenu, window.Canvas)

// Button to show menu at position
let showMenuBtn = widget.NewButton("Show Context Menu", () => {
	popupMenu.ShowAtPosition(200, 200)
	statusLabel.SetText("Menu shown at position (200, 200)")
})

// Content area with instruction
let contentArea = container.NewCenter(
	container.NewVBox(
		widget.NewLabel("Click the button above to show the context menu"),
		widget.NewLabel("(Note: Right-click detection requires additional event handling)")
	)
)

// Button to toggle menu item states
let toggleBtn = widget.NewButton("Toggle Cut & Find State", () => {
	cutItem.Disabled = !cutItem.Disabled
	findItem.Checked = !findItem.Checked
	statusLabel.SetText(sprintf("Cut disabled: %s, Find checked: %s",
		cutItem.Disabled, findItem.Checked))
})

// Info section
let infoLabel = widget.NewLabel(`Context Menu Features:
- Menu items with callbacks
- Separators for grouping
- Disabled items (Cut starts enabled)
- Checked items (checkmark indicator)
- Position control`)

let layout = container.NewBorder(
	container.NewVBox(
		infoLabel,
		widget.NewSeparator(),
		showMenuBtn,
		toggleBtn,
		widget.NewSeparator()
	),
	statusLabel,
	nil,
	nil,
	contentArea
)

window.SetContent(layout)
```

## Running

```bash
cd examples/23-menu
go run main.go
```

Or with the fynerisor CLI:

```bash
fynerisor script.risor
```

## Full Source

[View full example on GitHub ↗](https://github.com/uidbz/fynerisor/tree/main/examples/23-menu)

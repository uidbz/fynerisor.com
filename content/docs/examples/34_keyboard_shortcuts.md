---
title: Keyboard Shortcuts
type: docs
weight: 34
---

This example demonstrates keyboard shortcut support in fynerisor.

## Features

- **Global shortcuts**: Register keyboard shortcuts with `window.AddShortcut()`
- **Works without menus**: Shortcuts work immediately, no menu required
- **Multi-modifier shortcuts**: Use combinations like "Ctrl+Shift+N"
- **Special keys**: Function keys like F1
- **Dynamic management**: Add and remove shortcuts at runtime

## Code

```js
require(["v0.6"])

// Create a label to show keyboard shortcut status
let statusLabel = widget.NewLabel("Press Ctrl+S to save, Ctrl+O to open, Ctrl+Q to quit")
let outputLabel = widget.NewLabel("")

// Register global shortcuts (work without menus)
window.AddShortcut("Ctrl+S", () => {
    outputLabel.SetText("Save triggered! (Ctrl+S)")
    print("Save action triggered")
})

window.AddShortcut("Ctrl+O", () => {
    outputLabel.SetText("Open triggered! (Ctrl+O)")
    print("Open action triggered")
})

window.AddShortcut("Ctrl+Q", () => {
    outputLabel.SetText("Quit triggered! (Ctrl+Q)")
    print("Quit action triggered")
})

window.AddShortcut("Ctrl+Shift+N", () => {
    outputLabel.SetText("New window triggered! (Ctrl+Shift+N)")
    print("New window action triggered")
})

window.AddShortcut("F1", () => {
    outputLabel.SetText("Help triggered! (F1)")
    print("Help action triggered")
})

// Create a button to remove a shortcut
let removeBtn = widget.NewButton("Remove Ctrl+S Shortcut", () => {
    window.RemoveShortcut("Ctrl+S")
    outputLabel.SetText("Ctrl+S shortcut removed")
})

let addBtn = widget.NewButton("Add Ctrl+S Back", () => {
    window.AddShortcut("Ctrl+S", () => {
        outputLabel.SetText("Save triggered! (Ctrl+S)")
        print("Save action triggered")
    })
    outputLabel.SetText("Ctrl+S shortcut added back")
})

// Build UI
let content = container.NewVBox([
    statusLabel,
    widget.NewSeparator(),
    outputLabel,
    widget.NewSeparator(),
    removeBtn,
    addBtn
])

window.SetContent(content)
```

## Running

```bash
cd examples/34-keyboard-shortcuts
go run main.go
```

Or with the fynerisor CLI:

```bash
fynerisor script.risor
```

## Full Source

[View full example on GitHub ↗](https://github.com/uidbz/fynerisor/tree/main/examples/34-keyboard-shortcuts)

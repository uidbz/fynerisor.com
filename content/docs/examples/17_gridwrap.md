---
title: Grid Wrap
type: docs
weight: 17
---

Demonstrates the GridWrap widget - a grid layout with virtualized rendering for efficiently displaying many items.

![Grid Wrap Screenshot](/images/examples/17-gridwrap.png)

## Features

- **Grid Layout**: Automatically arranges items in a grid
- **Virtualized Rendering**: Only renders visible items for performance
- **Selection**: Click to select/deselect items
- **Callbacks**: Length(), CreateItem(), UpdateItem()
- **Events**: OnSelected(), OnUnselected()

## Code

```js
require(["v0.2", "@gui"])

// Sample data - image gallery
let images = [
    "Item 1", "Item 2", "Item 3", "Item 4",
    "Item 5", "Item 6", "Item 7", "Item 8",
    "Item 9", "Item 10", "Item 11", "Item 12"
]

let statusLabel = widget.NewLabel("Click an item to select")

let grid = widget.NewGridWrap()

// Set data length
grid.Length(() => {
    return len(images)
})

// Create template item
// Using buttons provides consistent sizing and interactivity
grid.CreateItem(() => {
    let btn = widget.NewButton("Template", () => {})
    btn.Importance = constants.ImportanceLow
    return btn
})

// Update item with data
grid.UpdateItem((id, item) => {
    item.SetText(images[id])
})

// Handle selection
grid.OnSelected((id) => {
    statusLabel.SetText(sprintf("Selected: %s", images[id]))
})

// Handle deselection
grid.OnUnselected((id) => {
    statusLabel.SetText("Item deselected")
})

// Refresh to display data
grid.Refresh()

let layout = container.NewBorder(
    statusLabel,  // top
    nil,          // bottom
    nil,          // left
    nil,          // right
    grid          // center
)

window.SetContent(layout)
```

## Running

```bash
cd examples/17-gridwrap
go run main.go
```

Or with the fynerisor CLI:

```bash
fynerisor script.risor
```

## Full Source

[View full example on GitHub ↗](https://github.com/uidbz/fynerisor/tree/main/examples/17-gridwrap)

---
title: List Widget
type: docs
weight: 5
---

Scrolling list with dynamic items.

![List Widget Example](/images/examples/11-list.png)

## Code

```js
require(["v0.4", "@gui"])

let items = [
    "First Item",
    "Second Item", 
    "Third Item",
    "Fourth Item",
    "Fifth Item",
]

let selectedLabel = widget.NewLabel("No selection")

let list = widget.NewList()

// Return number of items
list.Length(() => items.length)

// Create template item
list.CreateItem(() => widget.NewLabel(""))

// Update item with data
list.UpdateItem((id, obj) => {
    obj.Text = items[id]
})

// Handle selection
list.OnSelected((id) => {
    selectedLabel.Text = sprintf("Selected: %s", items[id])
})

let addButton = widget.NewButton("Add Item", () => {
    let newItem = sprintf("Item %d", items.length + 1)
    items = items.concat([newItem])
    list.Refresh()
})

let content = container.NewBorder(
    selectedLabel,
    addButton,
    nil,
    nil,
    list
)

window.SetContent(content)
```

## Explanation

1. **List Widget** - Efficient scrolling list with `widget.NewList()`
2. **Length Callback** - Returns total number of items
3. **CreateItem** - Creates template widget (reused for all items)
4. **UpdateItem** - Updates template with actual data for index
5. **OnSelected** - Callback when user selects an item
6. **Dynamic Updates** - Modify array and call `list.Refresh()`

## Key Concepts

- **Virtualization**: List reuses widgets for performance
- **Template Pattern**: CreateItem makes template, UpdateItem fills it
- **Selection**: Track selected items with OnSelected callback
- **Border Layout**: Use `container.NewBorder()` for complex layouts

## Try It

```bash
fynerisor list-widget.risor
```

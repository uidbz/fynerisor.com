---
title: Containers
type: docs
weight: 2
---

Containers organize and layout multiple widgets. Fynerisor v0.6.0 provides **10 container types** with 100% coverage.

All examples use **Risor v2 syntax** with `let` for variables.

## VBox

Vertical box - stacks widgets vertically from top to bottom.

```js
let vbox = container.NewVBox(
    widget.NewLabel("Top"),
    widget.NewLabel("Middle"),
    widget.NewLabel("Bottom")
)
```

**Use cases**: Vertical layouts, forms, lists

---

## HBox

Horizontal box - arranges widgets horizontally from left to right.

```js
let hbox = container.NewHBox(
    widget.NewLabel("Left"),
    widget.NewLabel("Center"),
    widget.NewLabel("Right")
)
```

**Use cases**: Toolbars, button groups, horizontal layouts

---

## Border

Border layout with top, bottom, left, right, and center regions. All parameters except center are optional (use `nil` for empty regions).

```js
let border = container.NewBorder(
    widget.NewLabel("Top"),      // top
    widget.NewLabel("Bottom"),   // bottom
    widget.NewLabel("Left"),     // left
    widget.NewLabel("Right"),    // right
    widget.NewLabel("Center")    // center (main content)
)

// Example with only top and center
let border = container.NewBorder(
    widget.NewLabel("Header"),   // top
    nil,                         // bottom
    nil,                         // left
    nil,                         // right
    mainContent                  // center
)
```

**Parameters** (in order):
1. `top` - Widget for top region (or nil)
2. `bottom` - Widget for bottom region (or nil)
3. `left` - Widget for left region (or nil)
4. `right` - Widget for right region (or nil)
5. `center` - Widget for center region (required)

**Use cases**: Application layouts with headers/footers, sidebars, main content areas

---

## Grid

Fixed grid layout with uniform cell sizes.

```js
let grid = container.NewGrid(2,  // 2 columns
    widget.NewLabel("Cell 1"),
    widget.NewLabel("Cell 2"),
    widget.NewLabel("Cell 3"),
    widget.NewLabel("Cell 4")
)
```

**Use cases**: Button grids, calculator layouts, uniform item grids

---

## GridWrap

Grid that wraps items based on size.

```js
let items = [
    widget.NewButton("1", () => {}),
    widget.NewButton("2", () => {}),
    widget.NewButton("3", () => {})
]

let gridWrap = container.NewGridWrap(items...)
```

**Use cases**: Responsive grids, image galleries, icon grids

---

## HSplit

Horizontal split container with draggable divider.

```js
let split = container.NewHSplit(
    widget.NewLabel("Left pane"),
    widget.NewLabel("Right pane")
)
split.SetOffset(0.3)  // 30% left, 70% right
```

**Methods**:
- `SetOffset(float)`: Set divider position (0.0 to 1.0)

**Use cases**: Side-by-side views, navigation + content, master-detail layouts

---

## VSplit

Vertical split container with draggable divider.

```js
let split = container.NewVSplit(
    widget.NewLabel("Top pane"),
    widget.NewLabel("Bottom pane")
)
split.SetOffset(0.5)  // 50/50 split
```

**Methods**:
- `SetOffset(float)`: Set divider position (0.0 to 1.0)

**Use cases**: Stacked views, code + output, resizable sections

---

## Scroll

Scrollable container for content larger than viewport.

```js
// Create content that may be larger than screen
let content = container.NewVBox(
    widget.NewLabel("Item 1"),
    widget.NewLabel("Item 2"),
    // ... many more items
    widget.NewLabel("Item 100")
)

let scrollable = container.NewScroll(content)
window.SetContent(scrollable)
```

**Important**: Unlike VBox/HBox which take multiple items, Scroll takes a single container as its argument.

**Use cases**: Long lists, large forms, content that exceeds screen size

---

## Center

Centers its content in the available space.

```js
let centered = container.NewCenter(
    widget.NewButton("I'm Centered", () => {})
)
```

**Use cases**: Centering dialogs, splash screens, focused content

---

## Max

Expands content to fill all available space.

```js
let max = container.NewMax(
    widget.NewLabel("Fills entire space")
)
```

**Use cases**: Full-screen content, background widgets

---

## Padded

Adds padding around content.

```js
let padded = container.NewPadded(
    widget.NewLabel("Padded content")
)
```

**Use cases**: Spacing around content, margins

---

## Stack

Stacks widgets on top of each other (Z-axis layering).

```js
let stack = container.NewStack(
    widget.NewLabel("Background"),  // Back
    widget.NewLabel("Middle"),
    widget.NewLabel("Front")        // Front
)
```

**Use cases**: Overlays, layered content, backgrounds

---

## Layout Tips

### Nesting Containers

Combine containers to create complex layouts:

```js
// Header with title and button
let header = container.NewHBox(
    widget.NewLabel("My App"),
    widget.NewButton("Help", () => {})
)

// Main content area
let content = container.NewVBox(
    widget.NewLabel("Welcome"),
    widget.NewLabel("More content...")
)

// Combine with border layout
let app = container.NewBorder(
    header,
    nil, nil, nil,
    content
)

window.SetContent(app)
```

### Scrollable Content

Always wrap long content in a scroll container:

```js
let form = container.NewVBox(/* many fields */)
let scrollable = container.NewScroll(form)
window.SetContent(scrollable)
```

### Complex Layout Example

```js
// Sidebar navigation
let sidebar = container.NewVBox(
    widget.NewButton("Home", () => {}),
    widget.NewButton("Settings", () => {}),
    widget.NewButton("About", () => {})
)

// Main content with header and footer
let mainContent = container.NewBorder(
    widget.NewLabel("Page Title"),           // header
    widget.NewLabel("© 2026 Company"),       // footer
    nil, nil,
    container.NewScroll(                     // scrollable content
        container.NewVBox(
            widget.NewLabel("Content line 1"),
            widget.NewLabel("Content line 2")
        )
    )
)

// Overall layout
let app = container.NewHSplit(sidebar, mainContent)
app.SetOffset(0.2)  // 20% sidebar, 80% content

window.SetContent(app)
```

## Summary

Fynerisor v0.6.0 provides **10 container types** (100% coverage):

1. **VBox** - Vertical stack
2. **HBox** - Horizontal stack
3. **Border** - 5-region layout
4. **Grid** - Fixed grid
5. **GridWrap** - Wrapping grid
6. **HSplit** - Horizontal split pane
7. **VSplit** - Vertical split pane
8. **Scroll** - Scrollable content
9. **Center** - Centered content
10. **Max** - Fill space
11. **Padded** - Add padding
12. **Stack** - Z-axis layering

All containers support nested composition for complex layouts.

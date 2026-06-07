---
title: Quick Reference
type: docs
weight: 7
---

Quick reference cheat sheet for Fynerisor v0.5.0 development with **Risor v2 syntax**.

## Basic Widgets

| Widget | Example |
|--------|---------|
| Button | `widget.NewButton("Click", () => {})` |
| Label | `widget.NewLabel("Hello")` |
| Entry | `widget.NewEntry()` |
| MultiLineEntry | `widget.NewMultiLineEntry()` |
| Check | `widget.NewCheck("Enable", (checked) => {})` |
| Select | `widget.NewSelect(["A", "B"], (val) => {})` |
| Slider | `widget.NewSlider(0, 100, (val) => {})` |
| ProgressBar | `widget.NewProgressBar()` |

## Data Binding

```js
let name = binding.NewString("John")
let label = widget.NewLabelWithData(name)
name.Set("Jane")  // Auto-updates UI
```

**Types**: `NewString`, `NewBool`, `NewInt`, `NewFloat`

## Containers

| Container | Example |
|-----------|---------|
| VBox | `container.NewVBox(w1, w2, w3)` |
| HBox | `container.NewHBox(w1, w2, w3)` |
| Border | `container.NewBorder(top, bottom, left, right, center)` |
| Grid | `container.NewGrid(2, w1, w2, w3, w4)` |
| Scroll | `container.NewScroll(content)` |
| HSplit | `container.NewHSplit(left, right)` |
| VSplit | `container.NewVSplit(top, bottom)` |

## Window Object

```js
window.SetContent(widget)
window.SetStatus("text")
window.Title = "My App"  // Get or set window title
window.Do(() => { /* update GUI from background */ })
window.OnDropped((paths) => { /* handle file drops */ })
```

**Properties**: `window.DroppedPaths` (array of dropped file paths)

## HTTP Module

```js
require("@http")

let response = http.get("https://api.example.com/data")
print(response.body)

let response = http.post(url, {"key": "value"})
```

## SQL Module

```js
require("@sql")

let conn = sql.connect("sqlserver://user:pass@server/db")
let rows = conn.query("SELECT * FROM Users WHERE id = ?", 123)
rows.each(row => print(row["name"]))
conn.close()
```

## Arrow Functions

```js
// Single expression
let double = x => x * 2

// Block
let process = x => {
    let result = x * 2
    return result + 1
}

// No params
let onClick = () => print("Clicked")
```

## Iteration

```js
// Each
[1, 2, 3].each(x => print(x))

// Map
let doubled = [1, 2, 3].map(x => x * 2)

// Filter
let evens = [1, 2, 3, 4].filter(x => x % 2 == 0)
```

## Background Tasks

```js
go(() => {
    let result = heavyWork()
    
    window.Do(() => {
        label.SetText(result)
    })
})
```

**Rule**: Always use `window.Do()` to update GUI from `go()` blocks.

## Module Loading

```js
require("@gui")
require(["v0.4", "@gui", "@http", "@sql"])

import("./utils.risor")
import("https://example.com/helpers.risor")
```

**Available Modules**: `@gui`, `@http`, `@sql`, `@os`, `@strings`, `@filepath`, `@time`, `@io`

## Common Patterns

**Form**:
```js
let entry = widget.NewEntry()
let form = widget.NewForm(
    widget.NewFormItem("Name:", entry)
)
```

**Table**:
```js
let table = widget.NewTable("Data", 20)
table.Columns(() => ["ID", "Name"])
table.Data((offset, limit) => data.slice(offset, limit))
```

**Progress**:
```js
let progress = widget.NewProgressBarInfinite()
progress.Start()
// Later: progress.Stop()
```

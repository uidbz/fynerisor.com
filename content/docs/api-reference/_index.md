---
title: API Reference
type: docs
weight: 3
sidebar:
  open: true
---

Complete reference documentation for the Fynerisor API.

## Quick Navigation

{{< cards >}}
  {{< card link="./widgets" title="Widgets" >}}
  {{< card link="./containers" title="Containers" >}}
  {{< card link="./canvas" title="Canvas Objects" >}}
  {{< card link="./gui" title="GUI Object" >}}
  {{< card link="./global-objects" title="Global Objects" >}}
{{< /cards >}}

## Overview

The fynerisor package provides comprehensive Risor bindings for Fyne UI components. All standard Fyne widgets and containers are available through simple factory functions.

### Basic Concepts

- **Widgets**: Interactive UI components (buttons, labels, entries, tables, etc.)
- **Containers**: Layout managers that organize widgets (VBox, HBox, Border, etc.)
- **Canvas Objects**: Low-level drawing primitives (images, lines)
- **Global Objects**: Built-in objects for window control, data binding, and modules

### Common Patterns

#### Creating a Simple UI

```js
require(["v0.4", "@gui"])

let label = widget.NewLabel("Hello")
let button = widget.NewButton("Click me", () => {
    print("Clicked!")
})

let vbox = container.NewVBox([label, button])
window.SetContent(vbox)
```

#### Reading Widget Properties

```js
let entry = widget.NewEntry()
entry.SetPlaceHolder("Enter text...")

// Later, read the value
let userInput = entry.Text
print("User entered:", userInput)
```

#### Using Modules

```js
require(["@http", "@sql"])

// HTTP request
let response = http.get("https://api.example.com/data")
let data = response.json()

// Database query
let conn = sql.connect("sqlite3::memory:")
let rows = conn.query("SELECT * FROM users")
rows.each(row => print(row["name"]))
```

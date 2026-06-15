---
title: Module Imports
type: docs
weight: 14
---

This example demonstrates the new module-scoped import system introduced in v0.6.0.

![Module Imports Screenshot](/images/examples/14-module-imports.png)

## Features

- **Module-scoped imports**: Use `let module = import("path")` to import modules with namespace isolation
- **Module caching**: Same import path returns the same module instance
- **Module functions**: Call functions from imported modules using dot notation
- **Module exports**: All top-level variables and functions in a module are automatically exported
- **UI integration**: Use module functions in callbacks and event handlers

## Code

```js
require(["v0.6", "@gui"])

// Test 1: Basic module import
print("=== Test 1: String Utils Module ===")
let strUtils = import("string_utils.risor")

let text = "Hello World"
print("Original:", text)
print("Upper:", strUtils.upper(text))
print("Lower:", strUtils.lower(text))
print("Reverse:", strUtils.reverse(text))

// Test 2: Module caching
print("\n=== Test 2: Module Caching ===")
let strUtils2 = import("string_utils.risor")
print("Same module instance?", strUtils == strUtils2)

// Test 2b: Module functions referencing module-level variables and functions
print("\n=== Test 2b: Module-Level References ===")
let math = import("math_utils.risor")
print("PI:", math.PI)
print("square(4):", math.square(4))
print("circleArea(2):", math.circleArea(2)) // PI * 2*2 = 12.56636
print("cube(3):", math.cube(3))              // 27

// Test 3: Using module in UI
print("\n=== Test 3: Module in UI ===")

let input = widget.NewEntry()
input.SetPlaceHolder("Enter text here...")
input.SetText("Fynerisor Modules")

let outputLabel = widget.NewLabel("")

let btnUpper = widget.NewButton("To Upper", () => {
    outputLabel.SetText(strUtils.upper(input.Text))
})

let btnLower = widget.NewButton("To Lower", () => {
    outputLabel.SetText(strUtils.lower(input.Text))
})

let btnReverse = widget.NewButton("Reverse", () => {
    outputLabel.SetText(strUtils.reverse(input.Text))
})

let buttons = container.NewHBox([btnUpper, btnLower, btnReverse])

let content = container.NewVBox([
    widget.NewLabel("Module Import Demo"),
    widget.NewSeparator(),
    input,
    buttons,
    widget.NewSeparator(),
    widget.NewLabel("Output:"),
    outputLabel
])

window.SetContent(content)
window.Resize(500, 300)
```

## Running

```bash
cd examples/14-module-imports
go run main.go
```

Or with the fynerisor CLI:

```bash
fynerisor script.risor
```

## Full Source

[View full example on GitHub ↗](https://github.com/uidbz/fynerisor/tree/main/examples/14-module-imports)

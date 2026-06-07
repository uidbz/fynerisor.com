---
title: Data Binding
type: docs
weight: 2
---

Data binding creates reactive UIs that automatically update when data changes.

## What is Data Binding?

Data binding connects widgets to data sources. When the data changes, the widget updates automatically without manual `SetText()` calls.

## Binding Types

### String Binding

```js
require("@gui")

let name = binding.NewString("John")
let label = widget.NewLabelWithData(name)

// Update data - label updates automatically
name.Set("Jane")

// Read current value
let current = name.Get()
```

### Bool Binding

```js
let enabled = binding.NewBool(false)
let checkbox = widget.NewCheckWithData("Enable feature", enabled)

// Listen for changes
enabled.AddListener(() => {
    print("Enabled:", enabled.Get())
})

// Toggle
enabled.Set(!enabled.Get())
```

### Int Binding

```js
let count = binding.NewInt(0)
let label = widget.NewLabelWithData(
    count.Map(x => sprintf("Count: %d", x))
)

// Increment
count.Set(count.Get() + 1)
```

### Float Binding

```js
let temperature = binding.NewFloat(20.5)
let slider = widget.NewSliderWithData(0, 100, temperature)
let label = widget.NewLabelWithData(
    temperature.Map(x => sprintf("%.1f°C", x))
)

// Slider changes automatically update label
```

## Binding Methods

All binding types support these methods:

- **`.Get()`** - Retrieve current value
- **`.Set(value)`** - Update value (triggers UI refresh)
- **`.AddListener(callback)`** - Execute callback on change
- **`.Map(fn)`** - Transform value for display

## Transform with Map

Use `.Map()` to format binding values:

```js
let price = binding.NewFloat(19.99)
let label = widget.NewLabelWithData(
    price.Map(x => sprintf("$%.2f", x))
)

let percent = binding.NewFloat(0.75)
let display = widget.NewLabelWithData(
    percent.Map(x => sprintf("%.0f%%", x * 100))
)
```

## Complete Example: Counter App

```js
require("@gui")

let count = binding.NewInt(0)

let countLabel = widget.NewLabelWithData(
    count.Map(x => sprintf("Count: %d", x))
)

let incrementBtn = widget.NewButton("Increment", () => {
    count.Set(count.Get() + 1)
})

let decrementBtn = widget.NewButton("Decrement", () => {
    count.Set(count.Get() - 1)
})

let resetBtn = widget.NewButton("Reset", () => {
    count.Set(0)
})

window.SetContent(container.NewVBox(
    countLabel,
    container.NewHBox(incrementBtn, decrementBtn, resetBtn)
))
```

## Two-Way Binding

Widgets with data binding automatically sync in both directions:

```js
let text = binding.NewString("")
let entry = widget.NewEntryWithData(text)
let label = widget.NewLabelWithData(text)

// Typing in entry automatically updates label
// Programmatic changes update both
text.Set("Hello")  // Updates entry AND label
```

## Multiple Listeners

Attach multiple callbacks to one binding:

```js
let value = binding.NewInt(0)

value.AddListener(() => {
    print("Value changed to:", value.Get())
})

value.AddListener(() => {
    window.SetStatus(sprintf("Count: %d", value.Get()))
})

value.Set(5)  // Triggers both listeners
```

## Form with Binding

```js
require("@gui")

let name = binding.NewString("")
let email = binding.NewString("")
let subscribe = binding.NewBool(false)

let form = container.NewVBox(
    widget.NewFormItem("Name:", widget.NewEntryWithData(name)),
    widget.NewFormItem("Email:", widget.NewEntryWithData(email)),
    widget.NewCheckWithData("Subscribe", subscribe),
    widget.NewButton("Submit", () => {
        print("Name:", name.Get())
        print("Email:", email.Get())
        print("Subscribe:", subscribe.Get())
    })
)

window.SetContent(form)
```

## When to Use Binding

**Use data binding when**:
- Multiple widgets display the same data
- UI should update automatically on data change
- Building reactive forms
- Synchronizing related widgets

**Use direct methods when**:
- One-time widget updates
- Simple static UIs
- Performance-critical tight loops

## Summary

Data binding provides:
- Automatic UI updates
- Two-way synchronization
- Clean separation of data and UI
- Multiple listener support
- Value transformation with `.Map()`

Available types: `String`, `Bool`, `Int`, `Float`

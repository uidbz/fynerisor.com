---
title: Data Binding Example
type: docs
weight: 11
---

Complete example showing reactive UI with data binding.

![Data Binding Example](/images/examples/25-data-binding.png)

## Counter with Binding

```js
require("@gui")

let count = binding.NewInt(0)

// Label automatically updates when count changes
let label = widget.NewLabelWithData(
    count.Map(x => sprintf("Count: %d", x))
)

let incrementBtn = widget.NewButton("+", () => {
    count.Set(count.Get() + 1)
})

let decrementBtn = widget.NewButton("-", () => {
    count.Set(count.Get() - 1)
})

let resetBtn = widget.NewButton("Reset", () => {
    count.Set(0)
})

window.SetContent(container.NewVBox(
    label,
    container.NewHBox(incrementBtn, decrementBtn, resetBtn)
))
```

## Form with Binding

```js
require("@gui")

let firstName = binding.NewString("")
let lastName = binding.NewString("")
let age = binding.NewInt(0)
let subscribe = binding.NewBool(false)

// Display updates automatically
let summary = widget.NewLabelWithData(
    firstName.Map(f => {
        let l = lastName.Get()
        let a = age.Get()
        return sprintf("Name: %s %s, Age: %d", f, l, a)
    })
)

// Add listeners for other bindings
lastName.AddListener(() => firstName.Set(firstName.Get()))
age.AddListener(() => firstName.Set(firstName.Get()))

let form = container.NewVBox(
    widget.NewLabel("User Registration"),
    widget.NewFormItem("First Name:", widget.NewEntryWithData(firstName)),
    widget.NewFormItem("Last Name:", widget.NewEntryWithData(lastName)),
    widget.NewFormItem("Age:", widget.NewEntryWithData(
        age.Map(x => sprintf("%d", x))
    )),
    widget.NewCheckWithData("Subscribe to newsletter", subscribe),
    container.NewVBox(
        widget.NewLabel(""),
        widget.NewLabel("Live Preview:"),
        summary
    )
)

window.SetContent(container.NewScroll(form))
```

## Slider with Labels

```js
require("@gui")

let value = binding.NewFloat(50.0)

let slider = widget.NewSliderWithData(0, 100, value)

let valueLabel = widget.NewLabelWithData(
    value.Map(x => sprintf("Value: %.1f", x))
)

let percentLabel = widget.NewLabelWithData(
    value.Map(x => sprintf("%.0f%%", x))
)

let bar = widget.NewProgressBarWithData(
    value.Map(x => x / 100.0)
)

window.SetContent(container.NewVBox(
    valueLabel,
    slider,
    percentLabel,
    bar
))
```

## Key Concepts

1. **Reactive Updates**: UI updates automatically when binding changes
2. **Two-Way Sync**: Entry widgets update binding and vice versa
3. **Transform with Map**: Format values for display
4. **Multiple Widgets**: One binding can drive multiple UI elements

## Run This Example

Save as `data-binding.risor` and run:
```bash
fynerisor data-binding.risor
```

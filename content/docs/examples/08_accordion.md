---
title: Accordion
type: docs
weight: 8
---

Demonstrates collapsible sections with expandable/collapsible accordion items.

![Accordion Screenshot](/images/examples/08-accordion.png)

## Features

- Shows **Accordion**: widget for collapsible sections
- Demonstrates single-open mode (default) vs multi-open mode
- Shows programmatic Open/Close/CloseAll control
- Demonstrates dynamic item management (Append, Remove, RemoveIndex)
- Shows rich content in accordion items (forms, buttons, other widgets)

## Code

```js
// Accordion Widget Example
// Demonstrates accordion with multiple sections

require("v0.2")

// Create title
let title = widget.NewLabel("Accordion Widget Demo")
title.TextStyle.Bold = true

// Basic accordion with three items
let item1 = widget.NewAccordionItem(
    "Getting Started",
    "",
    widget.NewLabel("Welcome! This section explains how to get started.")
)

let item2 = widget.NewAccordionItem(
    "Configuration",
    "",
    container.NewVBox(
        widget.NewLabel("Configuration options:"),
        widget.NewCheck("Enable feature A", (checked) => {
            print("Feature A toggled:", checked)
        }),
        widget.NewCheck("Enable feature B", (checked) => {
            print("Feature B toggled:", checked)
        })
    )
)

let item3 = widget.NewAccordionItem(
    "Advanced Settings",
    "",
    widget.NewLabel("Advanced settings for experienced users.")
)

let accordion = widget.NewAccordion(item1, item2, item3)

// Multi-open accordion
let multiItem1 = widget.NewAccordionItem("Section 1", "", widget.NewLabel("First section"))
let multiItem2 = widget.NewAccordionItem("Section 2", "", widget.NewLabel("Second section"))
let multiItem3 = widget.NewAccordionItem("Section 3", "", widget.NewLabel("Third section"))

let multiAccordion = widget.NewAccordion(multiItem1, multiItem2, multiItem3)
multiAccordion.MultiOpen = true

// Dynamic control buttons
let dynItem1 = widget.NewAccordionItem("Item 1", "", widget.NewLabel("First item"))
let dynItem2 = widget.NewAccordionItem("Item 2", "", widget.NewLabel("Second item"))
let dynItem3 = widget.NewAccordionItem("Item 3", "", widget.NewLabel("Third item"))

let dynamicAccordion = widget.NewAccordion(dynItem1, dynItem2, dynItem3)

let openFirstBtn = widget.NewButton("Open First", () => {
    dynamicAccordion.Open(0)
})

let closeFirstBtn = widget.NewButton("Close First", () => {
    dynamicAccordion.Close(0)
})

let closeAllBtn = widget.NewButton("Close All", () => {
    dynamicAccordion.CloseAll()
})

// Main layout
let content = container.NewVBox(
    title,
    widget.NewSeparator(),
    widget.NewLabel("Basic Accordion:"),
    accordion,
    widget.NewSeparator(),
    widget.NewLabel("Multi-Open Accordion:"),
    multiAccordion,
    widget.NewSeparator(),
    widget.NewLabel("Dynamic Control:"),
    dynamicAccordion,
    container.NewHBox(openFirstBtn, closeFirstBtn, closeAllBtn)
)

let scrollable = container.NewScroll(content)
window.SetContent(scrollable)
```

## Running

```bash
cd examples/08-accordion
go run main.go
```

Or with the fynerisor CLI:

```bash
fynerisor script.risor
```

## Full Source

[View full example on GitHub ↗](https://github.com/uidbz/fynerisor/tree/main/examples/08-accordion)

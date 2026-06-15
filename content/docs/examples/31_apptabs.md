---
title: App Tabs
type: docs
weight: 31
---

This example demonstrates the AppTabs container, which provides tabbed navigation for your application.

## Features

- **TabItem**: - Represents a single tab with text label and content
- **AppTabs**: - Container that manages multiple TabItem instances
- **Dynamic Management**: - Tabs can be added, removed, enabled, and disabled at runtime
- **Callbacks**: - Tab selection can trigger status updates or other actions
- **Layout Integration**: - Tabs work with other containers (Border, VBox, etc.)

## Code

```js
require(["v0.6"])

// Create content for tab 1
let label1 = widget.NewLabel("Welcome to Tab 1!")
let button1 = widget.NewButton("Button 1", () => {
    window.SetStatus("Clicked button in Tab 1")
})
let content1 = container.NewVBox([label1, button1])

// Create content for tab 2
let label2 = widget.NewLabel("This is Tab 2")
let entry2 = widget.NewEntry()
entry2.SetPlaceHolder("Enter text here...")
let content2 = container.NewVBox([label2, entry2])

// Create content for tab 3
let label3 = widget.NewLabel("Tab 3 with a list")
let items = ["Apple", "Banana", "Cherry", "Date"]
let list3 = widget.NewLabel("\n".join(items))
let content3 = container.NewVBox([label3, list3])

// Create tab items
let tab1 = container.NewTabItem("Home", content1)
let tab2 = container.NewTabItem("Input", content2)
let tab3 = container.NewTabItem("List", content3)

// Create AppTabs container with the tabs
let tabs = container.NewAppTabs(tab1, tab2, tab3)

// Create buttons to demonstrate tab control
let selectTab1Btn = widget.NewButton("Select Tab 1", () => {
    tabs.SelectIndex(0)
})

let selectTab2Btn = widget.NewButton("Select Tab 2", () => {
    tabs.SelectIndex(1)
})

let showSelectedBtn = widget.NewButton("Show Selected", () => {
    let index = tabs.SelectedIndex()
    window.SetStatus(sprintf("Currently selected tab index: %d", index))
})

let addTabBtn = widget.NewButton("Add New Tab", () => {
    let newLabel = widget.NewLabel("This is a dynamically added tab!")
    let newContent = container.NewVBox([newLabel])
    let newTab = container.NewTabItem("New Tab", newContent)
    tabs.Append(newTab)
    window.SetStatus("Added new tab")
})

let removeLastBtn = widget.NewButton("Remove Last Tab", () => {
    let index = tabs.SelectedIndex()
    if (index >= 0) {
        tabs.RemoveIndex(index)
        window.SetStatus(sprintf("Removed tab at index %d", index))
    }
})

let disableTab2Btn = widget.NewButton("Disable Tab 2", () => {
    tabs.DisableIndex(1)
    window.SetStatus("Disabled Tab 2")
})

let enableTab2Btn = widget.NewButton("Enable Tab 2", () => {
    tabs.EnableIndex(1)
    window.SetStatus("Enabled Tab 2")
})

// Create control panel
let controls = container.NewVBox([
    widget.NewLabel("Tab Controls:"),
    widget.NewSeparator(),
    selectTab1Btn,
    selectTab2Btn,
    showSelectedBtn,
    addTabBtn,
    removeLastBtn,
    disableTab2Btn,
    enableTab2Btn
])

// Create main layout with tabs and controls side by side
let mainLayout = container.NewBorder(nil, nil, nil, controls, tabs)

window.SetContent(mainLayout)
window.Resize(700, 400)
```

## Running

```bash
cd examples/31-apptabs
go run main.go
```

Or with the fynerisor CLI:

```bash
fynerisor script.risor
```

## Full Source

[View full example on GitHub ↗](https://github.com/uidbz/fynerisor/tree/main/examples/31-apptabs)

---
title: Widgets
type: docs
weight: 1
---

Widgets are interactive UI components. Fynerisor v0.6.0 provides **48+ widgets** with 100% fynerisor coverage.

All examples use **Risor v2 syntax** with arrow functions.

## Basic Widgets

### Button

Creates a clickable button with a callback function.

```js
let btn = widget.NewButton("Click Me", () => {
    print("Button clicked!")
})
```

**Button with Importance:**
```js
let highBtn = widget.NewButtonWithImportance("High", "HighImportance", () => {
    print("High priority action!")
})

let dangerBtn = widget.NewButtonWithImportance("Delete", "DangerImportance", () => {
    print("Danger!")
})
```

**Importance levels**: `HighImportance`, `LowImportance`, `MediumImportance`, `DangerImportance`, `WarningImportance`, `SuccessImportance`

**Icon Button:**
```js
let iconBtn = widget.NewButtonWithIcon("Save", "DocumentSaveIcon", () => {
    print("Saved!")
})
```

**Use cases**: Form submissions, navigation, triggering actions

---

### Label

Displays static or dynamic text.

```js
let label = widget.NewLabel("Initial text")
label.SetText("Updated text")
```

**With Data Binding:**
```js
let data = binding.NewString("Hello")
let label = widget.NewLabelWithData(data)
data.Set("Updated!") // Label updates automatically
```

**Properties**:
- `Text` (string): The displayed text (via SetText/GetText)

**Use cases**: Displaying information, status messages, headings

---

### Entry

Single-line text input field.

```js
let entry = widget.NewEntry()
entry.SetText("Default value")

entry.OnSubmitted(() => {
    print("Submitted:", entry.Text)
})
```

**With Data Binding:**
```js
let text = binding.NewString("")
let entry = widget.NewEntryWithData(text)
```

**With Password:**
```js
let password = widget.NewPasswordEntry()
```

**Properties**:
- `Text` (string): The current text value
- `PlaceHolder` (string): Placeholder text

**Methods**:
- `SetText(text)`: Set text
- `SetPlaceHolder(text)`: Set placeholder
- `OnSubmitted(callback)`: Called when user presses Enter

**Use cases**: Text input, search boxes, form fields

---

### MultiLineEntry

Multi-line text input area.

```js
let textArea = widget.NewMultiLineEntry()
textArea.SetText("Line 1\nLine 2\nLine 3")
```

**Properties**:
- `Text` (string): The current text value

**Use cases**: Long text input, comments, notes, code editing

---

### SelectEntry

Combination of dropdown and text entry.

```js
let options = ["Option 1", "Option 2", "Option 3"]
let selectEntry = widget.NewSelectEntry(options)
selectEntry.SetText("Custom value")
```

**Use cases**: Autocomplete, combo boxes with custom values

---

### Check

Checkbox with change callback.

```js
let check = widget.NewCheck("Enable feature", (checked) => {
    print("Checked:", checked)
})
check.SetChecked(true)
```

**With Data Binding:**
```js
let checked = binding.NewBool(false)
let check = widget.NewCheckWithData("Enable", checked)
```

**Properties**:
- `Checked` (bool): Current checkbox state

**Methods**:
- `SetChecked(bool)`: Set the checkbox state

**Use cases**: Boolean options, feature toggles, selections

---

### CheckGroup

Group of checkboxes with multi-selection.

```js
let options = ["Option 1", "Option 2", "Option 3"]
let checkGroup = widget.NewCheckGroup(options, (selected) => {
    print("Selected:", selected)  // Returns list of checked items
})
```

**Use cases**: Multiple selections, filtering options

---

### Radio

Radio button group (single selection).

```js
let radio = widget.NewRadio("Option A", (selected) => {
    print("Selected:", selected)
})
```

**Use cases**: Single selection from a group

---

### RadioGroup

Group of radio buttons.

```js
let options = ["Small", "Medium", "Large"]
let radioGroup = widget.NewRadioGroup(options, (selected) => {
    print("Size selected:", selected)
})
radioGroup.SetSelected("Medium")
```

**Methods**:
- `SetSelected(value)`: Set selected option

**Use cases**: Single choice from multiple options

---

### Select

Dropdown selection widget.

```js
let options = ["Red", "Green", "Blue"]
let dropdown = widget.NewSelect(options, (selected) => {
    print("Selected color:", selected)
})
dropdown.SetSelectedIndex(0)  // Select first option
```

**Methods**:
- `SetSelectedIndex(index)`: Select option by index
- `SetSelected(value)`: Select by value

**Use cases**: Single choice from multiple options, category selection

---

### Slider

Slider for numeric value selection.

```js
let slider = widget.NewSlider(0, 100, (value) => {
    print("Value:", value)
})
slider.SetValue(50)
```

**With Data Binding:**
```js
let value = binding.NewFloat(50.0)
let slider = widget.NewSliderWithData(0, 100, value)
```

**Methods**:
- `SetValue(float)`: Set slider value

**Use cases**: Volume controls, opacity, numeric ranges

---

### ProgressBar

Progress bar indicator.

```js
let progress = widget.NewProgressBar()
progress.SetValue(0.5)  // 50%
```

**With Data Binding:**
```js
let value = binding.NewFloat(0.0)
let progress = widget.NewProgressBarWithData(value)
```

**Infinite Progress:**
```js
let progress = widget.NewProgressBarInfinite()
progress.Start()
// Later...
progress.Stop()
```

**Methods**:
- `SetValue(float)`: Set progress (0.0 to 1.0)
- `Start()`: Start infinite animation
- `Stop()`: Stop infinite animation

**Use cases**: Loading indicators, task progress

---

### ActivityIndicator

Spinning activity indicator.

```js
let activity = widget.NewActivity()
activity.Start()
// Later...
activity.Stop()
```

**Use cases**: Background operations, loading states

---

### Separator

Visual separator line.

```js
let separator = widget.NewSeparator()
```

**Use cases**: Dividing sections, visual grouping

---

### Icon

Display an icon from theme.

```js
let icon = widget.NewIcon("DocumentSaveIcon")
```

**Common Icons**: `DocumentSaveIcon`, `DocumentOpenIcon`, `DeleteIcon`, `SettingsIcon`, `HomeIcon`, `HelpIcon`, `InfoIcon`, `WarningIcon`, `ErrorIcon`, `ConfirmIcon`, `CancelIcon`, `SearchIcon`, `MenuIcon`

**Use cases**: Visual indicators, toolbar buttons

---

### Hyperlink

Clickable link to URL.

```js
let link = widget.NewHyperlink("Visit Site", "https://example.com")
```

**Use cases**: External links, documentation references

---

### Card

Card widget with title, subtitle, and content.

```js
let card = widget.NewCard(
    "Card Title",
    "Subtitle",
    widget.NewLabel("Card content goes here")
)
```

**With Image:**
```js
let img = canvas.NewImageFromFile("image.png")
let card = widget.NewCard("Title", "Subtitle", img)
```

**Use cases**: Information panels, dashboards, galleries

---

## Form Widgets

### Form

Form layout with labeled fields and submit/cancel buttons.

```js
let nameEntry = widget.NewEntry()
let emailEntry = widget.NewEntry()

let form = widget.NewForm(
    widget.NewFormItem("Name:", nameEntry),
    widget.NewFormItem("Email:", emailEntry)
)

form.OnSubmit(() => {
    print("Name:", nameEntry.Text)
    print("Email:", emailEntry.Text)
})

form.OnCancel(() => {
    print("Cancelled")
})
```

**Methods**:
- `OnSubmit(callback)`: Handle form submission
- `OnCancel(callback)`: Handle form cancellation

**Use cases**: Data entry forms, user input collection

---

### FormItem

Individual form field with label.

```js
let entry = widget.NewEntry()
let formItem = widget.NewFormItem("Username:", entry)
```

**Use cases**: Building custom forms

---

### DateEntry

Date picker widget.

```js
let dateEntry = widget.NewDateEntry((date) => {
    print("Selected date:", date)
})
```

**Use cases**: Date selection, scheduling

---

### Calendar

Calendar widget for date selection.

```js
let calendar = widget.NewCalendar((date) => {
    print("Selected:", date)
})
```

**Use cases**: Date pickers, scheduling interfaces

---

## Advanced Widgets

### Table

Advanced table widget with pagination and custom data binding.

```js
let table = widget.NewTable("My Table", 20)  // 20 rows per page

table.Columns(() => {
    return ["ID", "Name", "Status"]
})

table.RowCount(() => {
    return 100  // Total number of rows
})

table.Data((offset, limit) => {
    // Return slice of data for current page
    return data[offset:limit]
})

table.SetOnClick((row, col) => {
    print("Clicked row", row, "col", col)
})

table.SetColumnWidth(0, 150)  // Set column 0 width to 150px
table.Refresh()
```

**Alternative Cell-by-Cell Rendering:**
```js
table.CellValue((row, col) => {
    return sprintf("Cell %d,%d", row, col)
})
```

**Methods**:
- `Columns(callback)`: Returns array of column names
- `RowCount(callback)`: Returns total number of rows
- `Data(offset, limit)`: Returns data for current page
- `CellValue(row, col)`: Alternative cell-by-cell rendering
- `SetOnClick(callback)`: Handle cell clicks
- `SetColumnWidth(column, width)`: Set column width in pixels
- `Refresh()`: Update table display

**Use cases**: Data tables, database views, search results, reports

---

### List

Virtualized list widget for large datasets.

```js
let itemCount = 1000

let list = widget.NewList(
    () => itemCount,  // Length function
    () => widget.NewLabel("Template"),  // Template function
    (id, item) => {  // Update function
        item.SetText(sprintf("Item %d", id))
    }
)
```

**Use cases**: Large lists, file browsers, menu lists

---

### Tree

Tree widget with hierarchical data.

```js
let tree = widget.NewTree(
    (uid) => {  // ChildUIDs function
        if (uid == "") {
            return ["root1", "root2"]  // Root items
        }
        return [uid + "-child1", uid + "-child2"]
    },
    (uid) => uid != "",  // IsBranch function
    () => widget.NewLabel("Template"),  // Template function
    (uid, branch, item) => {  // Update function
        item.SetText(uid)
    }
)
```

**Use cases**: File trees, org charts, navigation menus

---

### Accordion

Collapsible accordion panels.

```js
let accordion = widget.NewAccordion(
    widget.NewAccordionItem("Section 1", widget.NewLabel("Content 1")),
    widget.NewAccordionItem("Section 2", widget.NewLabel("Content 2")),
    widget.NewAccordionItem("Section 3", widget.NewLabel("Content 3"))
)
```

**Use cases**: Collapsible sections, FAQs, settings panels

---

### Toolbar

Toolbar with action buttons.

```js
let toolbar = widget.NewToolbar(
    widget.NewToolbarAction("DocumentSaveIcon", () => print("Save")),
    widget.NewToolbarAction("DocumentOpenIcon", () => print("Open")),
    widget.NewToolbarSeparator(),
    widget.NewToolbarAction("DeleteIcon", () => print("Delete"))
)
```

**Use cases**: Application toolbars, action bars

---

### TextGrid

Monospace text grid for code/data display.

```js
let textGrid = widget.NewTextGrid()
textGrid.SetText("Line 1\nLine 2\nLine 3")
```

**Use cases**: Code display, logs, tabular text data

---

### RichText

Rich text widget with formatting.

```js
let richText = widget.NewRichTextFromMarkdown("# Title\n\nSome **bold** text")
```

**Use cases**: Formatted text, markdown rendering, documentation

---

### Log

Scrolling log widget with maximum item limit.

```js
let log = widget.NewLog(100)  // Keep last 100 entries
log.Append("Log entry 1")
log.Append("Log entry 2")
log.Append("Log entry 3")
```

**Methods**:
- `Append(text)`: Add new log entry

**Use cases**: Console output, debug logs, activity feeds

---

### Markdown

Renders markdown text with full CommonMark support.

```js
let md = widget.NewMarkdown("# Title\n\nSome **bold** text")
```

**Use cases**: Documentation, formatted text, help pages

---

### GridWrap

Grid layout that wraps items.

```js
let items = [
    widget.NewButton("1", () => {}),
    widget.NewButton("2", () => {}),
    widget.NewButton("3", () => {})
]

let gridWrap = widget.NewGridWrap(items...)
```

**Use cases**: Image galleries, icon grids, responsive layouts

---

## Charts

### BarChart

Bar chart widget for data visualization.

```js
let data = [5, 10, 15, 20, 15, 10, 5]
let chart = chart.NewBarChart("Sales Data", data)
```

**Use cases**: Data visualization, reports, dashboards

---

## Summary

Fynerisor v0.6.0 provides **48+ widgets** with 100% coverage:

- **High Priority** (11/11): Button, Label, Entry, Check, Select, Form, Table, ProgressBar, List, Calendar, DateEntry
- **Medium Priority** (13/13): MultiLineEntry, CheckGroup, RadioGroup, Slider, Icon, Hyperlink, Card, Tree, Accordion, Toolbar, Separator, ActivityIndicator, SelectEntry
- **Low Priority** (13/13): TextGrid, RichText, GridWrap, Log, Markdown, Radio, BarChart, etc.

All widgets support Risor v2 syntax with arrow functions and data binding where applicable.

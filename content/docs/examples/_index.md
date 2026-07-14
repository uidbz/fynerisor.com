---
title: Examples
type: docs
weight: 4
prev: docs/Examples
next: docs/Examples/hello-world
sidebar:
  open: true
---

Real-world examples and code snippets to help you build Fynerisor applications.

## All Examples

Browse all 35 examples demonstrating different widgets and features:

{{< cards >}}
  {{< card link="./01_hello_world" title="Hello World" >}}
  {{< card link="./02_button_callback" title="Button Callback" >}}
  {{< card link="./03_form_input" title="Form Input" >}}
  {{< card link="./04_table_display" title="Table Display" >}}
  {{< card link="./05_progress_widgets" title="Progress Widgets" >}}
  {{< card link="./06_icon_hyperlink_card" title="Icon, Hyperlink & Card" >}}
  {{< card link="./07_radiogroup" title="Radio Group" >}}
  {{< card link="./08_accordion" title="Accordion" >}}
  {{< card link="./09_toolbar" title="Toolbar" >}}
  {{< card link="./10_calendar" title="Calendar" >}}
  {{< card link="./11_list" title="List Widget" >}}
  {{< card link="./12_tree" title="Tree Widget" >}}
  {{< card link="./13_http_fetch" title="HTTP Fetch" >}}
  {{< card link="./14_module_imports" title="Module Imports" >}}
  {{< card link="./15_sql_test" title="SQL Database" >}}
  {{< card link="./16_context_builder" title="Context Builder" >}}
  {{< card link="./17_gridwrap" title="Grid Wrap" >}}
  {{< card link="./18_textgrid" title="Text Grid" >}}
  {{< card link="./19_richtext" title="Rich Text" >}}
  {{< card link="./20_button_importance" title="Button Importance" >}}
  {{< card link="./21_form_validation" title="Form Validation" >}}
  {{< card link="./22_popup" title="Popup Menu" >}}
  {{< card link="./23_menu" title="Menu" >}}
  {{< card link="./24_constants" title="Constants" >}}
  {{< card link="./25_data_binding" title="Data Binding" >}}
  {{< card link="./26_data_binding_types" title="Data Binding Types" >}}
  {{< card link="./27_app_versioning" title="App Versioning" >}}
  {{< card link="./28_custom_struct" title="Custom Struct" >}}
  {{< card link="./30_dialogs" title="Dialogs" >}}
  {{< card link="./31_apptabs" title="App Tabs" >}}
  {{< card link="./32_table_widgets" title="Table Widgets" >}}
  {{< card link="./33_image_gallery" title="Image Gallery" >}}
  {{< card link="./34_keyboard_shortcuts" title="Keyboard Shortcuts" >}}
  {{< card link="./35_charts" title="Charts" >}}
{{< /cards >}}

## Tips & Tricks

### String Manipulation

```js
require(["@strings"])

// Join array with separator
let text = strings.join(items, ", ")

// Split string
let parts = strings.split("a,b,c", ",")

// Upper/lower case
let upper = strings.upper("hello")
let lower = strings.lower("WORLD")
```

### Error Handling

```js
let result = someOperation()
if result != nil {
    window.SetStatus("Success!")
} else {
    window.SetStatus("Operation failed")
}
```

### Platform Detection

```js
require(["@os"])

let platform = os.goos()
if platform == "windows" {
    // Windows-specific code
} else if platform == "linux" {
    // Linux-specific code
}
```

### File Operations

```js
require(["@os", "@filepath"])

// Read directory
os.read_dir("/path/to/dir").each(entry => {
    print(entry.name, entry.is_dir, entry.size)
})

// Read/write files
os.write_file("output.txt", "Hello, World!")
let data = os.read_file("input.txt")
let text = string(data)
```

### Keyboard Shortcuts

```js
require(["v0.6"])

// Register global shortcuts
window.AddShortcut("Ctrl+S", () => {
    window.SetStatus("Saved!")
})

// Remove shortcuts dynamically
window.RemoveShortcut("Ctrl+S")
```

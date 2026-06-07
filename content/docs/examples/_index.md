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

## Quick Examples

{{< cards >}}
  {{< card link="./hello-world" title="Hello World" >}}
  {{< card link="./button-callback" title="Button Callback" >}}
  {{< card link="./form-input" title="Form Input" >}}
  {{< card link="./table-display" title="Table Display" >}}
  {{< card link="./list-widget" title="List Widget" >}}
  {{< card link="./tree-widget" title="Tree Widget" >}}
  {{< card link="./sql-database" title="SQL Database" >}}
  {{< card link="./http-fetch" title="HTTP Fetch" >}}
  {{< card link="./data-binding" title="Data Binding" >}}
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

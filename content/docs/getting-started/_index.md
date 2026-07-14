---
title: Getting Started
type: docs
weight: 1
sidebar:
  open: true
---

## What is Fynerisor?

Fynerisor provides Risor v2 script bindings for the Fyne GUI toolkit, enabling you to build cross-platform desktop applications using a simple scripting language. Whether you're creating tools, prototypes, or full applications, Fynerisor combines the power of Go with the simplicity of scripting.

Fynerisor can be used as a CLI tool to run scripts directly, or embedded as a library in your Go applications to provide scriptable interfaces. The package is split into `core` (headless) and `gui` (with UI) to support both static compilation and full desktop applications.

**Current Version:** **v0.7.0** (Production Ready)

## Key Features

### Script-based UI
- Build GUIs using Risor v2 scripts with modern arrow function syntax
- No compilation needed - just write and run
- **48+ widgets** available (comprehensive Fyne coverage)
- **Keyboard shortcuts** - Global shortcuts and menu integration
- Data binding for reactive UIs with automatic updates

### Dual Package Architecture
- **`gui` package** - Full GUI capabilities with Fyne (for desktop applications)
- **`core` package** - Headless execution without GUI dependencies (for CLI tools, static compilation)

### Rich UI Components
- 48+ widgets: buttons, forms, tables, charts, calendars, trees, lists, and more
- 10+ container types: VBox, HBox, Border, Grid, Split, Scroll, etc.
- Canvas objects for images and custom drawing
- Charts for data visualization: bar, line, scatter, histogram, and box plot
- Keyboard shortcuts and menu support
- Browser package for building navigable, script-driven applications

### Built-in Modules
- **HTTP** - REST API calls with JSON support
- **SQL** - Database connectivity (SQLite, PostgreSQL, MySQL, SQL Server)
- **OS** - System operations, file I/O, browser launching
- **Time** - Date/time handling
- **Strings** - String manipulation utilities
- **Filepath** - Path operations
- **IO** - File I/O operations

### Embeddable
- Use as a library in your Go applications
- Add custom global objects with `WithGlobal()`
- Set application version with `SetAppVersion()`
- Callbacks for status updates and results

### Cross-platform
- Runs on Windows, Linux, macOS, and Android
- Native desktop widgets
- Concurrency support with `go()` and `window.Do()`
- Import system for modular scripts

## Installation

### Prerequisites

For GUI applications, install the Fyne prerequisites for your platform:
- [Fyne Getting Started Guide](https://docs.fyne.io/started/quick/) - System dependencies

For headless (core) usage, no GUI dependencies are required.

### Install CLI Tool

```bash
go install github.com/uidbz/fynerisor/cmd/fynerisor@latest
```

### Install as Library

For GUI applications:
```bash
go get github.com/uidbz/fynerisor/gui
```

For headless/static compilation:
```bash
go get github.com/uidbz/fynerisor/core
```

## Running Fynerisor

### CLI Tool

```bash
# Run a script
fynerisor script.risor

# Watch mode (auto-reload on changes)
fynerisor --watch script.risor

# Custom window size
fynerisor --width 1024 --height 768 script.risor

# Headless mode (testing)
fynerisor --headless script.risor
```

### As a Library

See the [examples](#your-first-app) below for embedding Fynerisor in your Go application.

## Your First App

### Simple Script

Create a file called `hello.risor`:

```js
require(["v0.6", "@gui"])

let count = 0
let label = widget.NewLabel(sprintf("Count: %d", count))

let btn = widget.NewButton("Click Me", () => {
    count = count + 1
    label.SetText(sprintf("Count: %d", count))
})

window.SetContent(container.NewVBox([btn, label]))
```

**Run it:**
```bash
fynerisor hello.risor
```

### Embedded in Go

```go
package main

import "github.com/uidbz/fynerisor/gui"

func main() {
    fw := gui.NewApp("Counter App",
        gui.WithAppName("counter"),
    )
    
    fw.LoadScript(`
        require(["v0.6", "@gui"])
        
        let count = 0
        let label = widget.NewLabel(sprintf("Count: %d", count))
        
        let btn = widget.NewButton("Click Me", () => {
            count = count + 1
            label.SetText(sprintf("Count: %d", count))
        })
        
        window.SetContent(container.NewVBox([btn, label]))
    `)
    
    fw.Execute()
    fw.ShowAndRun()
}
```

{{< callout type="info" >}}
With the CLI tool and `--watch` flag, changes to your script appear immediately on file save.
{{< /callout >}}

## Risor v2 Syntax

goto uses Risor v2 with modern syntax:

### Arrow Functions
```js
// Single expression
let double = x => x * 2

// Block with multiple statements
let process = x => {
    let result = x * 2
    return result + 1
}

// No parameters
let onClick = () => print("Clicked!")

// Multiple parameters
let add = (a, b) => a + b
```

### Functional Iteration
No for/while loops - use functional methods:

```js
// Each
[1, 2, 3].each(x => print(x))

// Map
let doubled = [1, 2, 3].map(x => x * 2)

// Filter
let evens = [1, 2, 3, 4].filter(x => x % 2 == 0)

// Reduce
let sum = [1, 2, 3, 4].reduce((acc, x) => acc + x, 0)
```

### Module System
```js
// Require modules
require("@gui")
require(["@gui", "@http", "@sql"])

// Version requirements
require(["v1.0", "@gui"])

// Import other scripts
import("./utils.risor")
import("../shared/common.risor")
```

## Next Steps

{{< cards >}}
  {{< card link="../hello-world" title="Hello World Tutorial" >}}
  {{< card link="../api-reference" title="API Reference" >}}
  {{< card link="../examples" title="Code Examples" >}}
  {{< card link="../advanced" title="Advanced Topics" >}}
{{< /cards >}}

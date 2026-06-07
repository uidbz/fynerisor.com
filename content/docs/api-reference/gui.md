---
title: GUI / Window Object
type: docs
weight: 4
---

The global `gui` (also available as `window`) object provides application-level functionality and window control.

All examples use **Risor v2 syntax** with arrow functions.

## Methods

### SetContent

Sets the main content of the window.

```js
window.SetContent(myWidget)

// Or using gui
window.SetContent(myWidget)
```

**Use cases**: Display your UI, switch between views

---

### SetStatus

Updates the status bar text.

```js
window.SetStatus("Processing...")
window.SetStatus("Done!")
```

**Use cases**: Progress updates, status messages, user feedback

---

### Do

Execute a function on the main GUI thread. **Required when updating GUI from background goroutines.**

```js
go(() => {
    // Background work
    let result = heavyComputation()
    
    // Update GUI - must use window.Do()
    window.Do(() => {
        label.SetText(result)
    })
})
```

**Important**: Always use `window.Do()` when updating widgets from `go()` blocks.

**Use cases**: Updating UI from background tasks, thread-safe GUI updates

---

### Resize

Resize the window to specific dimensions.

```js
window.Resize(800, 600)  // width, height in pixels
```

**Use cases**: Setting window size, responsive layouts

---

### goto

Navigates to a different URL. Supports absolute and relative paths.

```js
// Absolute URL
gui.goto("https://goto.global.iff.com/app/myapp")

// Relative path (same directory)
gui.goto("./other-app.risor")
gui.goto("other-app.risor")

// Navigate up one level
gui.goto("..")

// Navigate to subdirectory
gui.goto("./subdir/app.risor")
```

**Use cases**: Navigation between apps, menu systems, workflow progression

---

### OpenBrowser

Opens a URL in the system's default web browser.

```js
gui.OpenBrowser("https://example.com")
gui.OpenBrowser("https://goto.global.iff.com/docs")
```

**Use cases**: External links, documentation, help pages

---

### OpenExcel

Opens an Excel file in the default spreadsheet application.

```js
gui.OpenExcel("/path/to/file.xlsx")
gui.OpenExcel("S:/data/report.xlsx")
```

**Use cases**: Opening generated reports, viewing data files

---

### OpenDir

Opens a directory in the system file explorer.

```js
gui.OpenDir("/path/to/directory")
gui.OpenDir("S:/shared/folder")
```

**Use cases**: Browse files, open containing folder

---

### OnDropped

Handles file drag-and-drop events.

```js
window.OnDropped(() => {
    let paths = window.DroppedPaths
    print("Dropped files:", paths)
    
    paths.each(path => {
        print("Processing:", path)
    })
})
```

**Use cases**: File upload, batch processing, drag-and-drop interfaces

---

### CaptureStdout

Captures stdout and sends it to a callback function.

```js
let log = widget.NewLog(100)
gui.CaptureStdout((line) => {
    log.Append(line)
})

// Now all print() statements will appear in the log widget
print("This appears in the log")
```

**Use cases**: Logging, debugging, capturing script output

---

## Properties

### GOOS

Operating system identifier.

```js
if (gui.GOOS == "windows") {
    // Windows-specific code
    let path = "C:\\Users\\..."
} else if (gui.GOOS == "linux") {
    // Linux-specific code
    let path = "/home/..."
} else if (gui.GOOS == "darwin") {
    // macOS-specific code
    let path = "/Users/..."
}
```

**Values**: `"windows"`, `"linux"`, `"darwin"` (macOS)

---

### Status

Current status text (read/write).

```js
gui.Status = "Loading..."
let currentStatus = gui.Status
```

---

### CurrentBaseURL

Current script's base URL. Useful for loading resources relative to the script.

```js
// Load image from same directory as script
let logo = canvas.NewImageFromURI(gui.CurrentBaseURL + "logo.png")

// Navigate to sibling script
gui.goto(gui.CurrentBaseURL + "other-app.risor")
```

---

### DroppedPaths

List of dropped file paths (updated by OnDropped).

```js
window.OnDropped(() => {
    let files = window.DroppedPaths
    files.each(file => {
        print("Processing:", file)
    })
})
```

---

## Common Patterns

### Background Task with Progress

```js
require("@gui")

let statusLabel = widget.NewLabel("Ready")
let progress = widget.NewProgressBarInfinite()
progress.Stop()

let btn = widget.NewButton("Start Task", () => {
    statusLabel.SetText("Processing...")
    progress.Start()
    
    // Run in background
    go(() => {
        let result = heavyComputation()
        
        // Update GUI on main thread
        window.Do(() => {
            progress.Stop()
            statusLabel.SetText("Done: " + result)
        })
    })
})

window.SetContent(container.NewVBox(
    btn,
    progress,
    statusLabel
))
```

### Platform-Specific Paths

```js
let defaultPath = ""
if (gui.GOOS == "windows") {
    defaultPath = "S:\\shared\\data"
} else {
    defaultPath = "/mnt/shared/data"
}
```

### Resource Loading

```js
// Load resources from same directory as script
let baseURL = gui.CurrentBaseURL
let logo = canvas.NewImageFromURI(baseURL + "logo.png")
```

### File Drop Handler

```js
let fileLabel = widget.NewLabel("Drop files here")

window.OnDropped(() => {
    let paths = window.DroppedPaths
    fileLabel.SetText("Files: " + paths.join(", "))
    
    paths.each(path => {
        print("Processing:", path)
        // Process each file
    })
})

window.SetContent(container.NewCenter(fileLabel))
```

### Loading Indicator

```js
window.SetStatus("Loading data...")

go(() => {
    let data = loadData()
    
    window.Do(() => {
        window.SetStatus("Ready")
        displayData(data)
    })
})
```

## Threading Rules

{{< callout type="warning" >}}
**Important**: GUI widgets can only be updated from the main thread. When using `go()` for background tasks, always wrap GUI updates in `window.Do()`:

```js
// ✓ Correct
go(() => {
    let result = backgroundWork()
    window.Do(() => {
        label.SetText(result)
    })
})

// ✗ Wrong - will crash
go(() => {
    let result = backgroundWork()
    label.SetText(result)  // Don't update GUI directly!
})
```
{{< /callout >}}

## Summary

The `gui`/`window` object provides:
- **Window control**: SetContent, Resize, SetStatus
- **Navigation**: goto, OpenBrowser, OpenExcel, OpenDir
- **File handling**: OnDropped, DroppedPaths
- **Threading**: Do() for thread-safe GUI updates
- **System info**: GOOS, CurrentBaseURL
- **Output capture**: CaptureStdout

Use `window.Do()` for all GUI updates from background goroutines.

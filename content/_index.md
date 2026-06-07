---
title: Build Desktop GUIs with Risor Scripts
toc: false
---

Fynerisor provides Risor v2 script bindings for the Fyne GUI toolkit, enabling you to create interactive desktop applications using a simple scripting language.

## Features

- 🎨 **34+ Widgets** - Buttons, forms, tables, trees, charts, and more
- 🔧 **Embeddable** - Use as a library in your Go applications  
- 📦 **Module System** - HTTP, SQL, OS, File I/O, Time, and String utilities
- 🔄 **Import System** - Load scripts from local files or HTTP(S) URLs
- 🧵 **Concurrency** - Background tasks with `go()` and safe GUI updates
- 📱 **Cross-Platform** - Linux, Windows, macOS, Android
- ⚡ **Static Compilation** - Headless mode with no GUI dependencies

## Example

<div style="text-align: center; margin: 2rem 0;">
  <img src="/images/fynerisor-screenshot.png" alt="Fynerisor Example" style="max-width: 700px; width: 100%; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
</div>

**Quick snippet** (simplified for brevity - [view full example on GitHub ↗](https://github.com/uidbz/fynerisor/blob/main/examples/hello.risor)):

```js
require(["v0.4", "@gui"])

let count = 0
let label = widget.NewLabel(sprintf("Count: %d", count))

let btn = widget.NewButton("Click Me", () => {
    count = count + 1
    label.SetText(sprintf("Count: %d", count))
})

let vbox = container.NewVBox([btn, label])
window.SetContent(vbox)
```

## Explore

{{< cards >}}
  {{< card link="docs" title="Documentation" icon="book-open" >}}
  {{< card link="download" title="Download" icon="download" >}}
  {{< card link="about" title="About" icon="user" >}}
{{< /cards >}}

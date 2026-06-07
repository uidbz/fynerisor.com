---
title: Hello World
type: docs
weight: 1
---

The simplest possible Fynerisor app.

![Hello World Example](/images/examples/01-hello-world.png)

## Code

```js
require(["v0.4", "@gui"])

let label = widget.NewLabel("Hello World")
window.SetContent(container.NewHBox([label]))
```

## Explanation

1. Declare version and module requirements with `require()`
2. Create a label widget with text "Hello World"
3. Place it in a horizontal box container
4. Set it as the window content using `window.SetContent()`

## Try It

Save this as `hello.risor` and run:

```bash
fynerisor hello.risor
```

Or use watch mode for automatic reloading:

```bash
fynerisor --watch hello.risor
```

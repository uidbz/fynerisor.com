---
title: Hello World
type: docs
weight: 1
---

The simplest possible fynerisor application. Demonstrates loading a Risor script from a file and creating a basic label widget.

![Hello World Screenshot](/images/examples/01-hello-world.png)

## Features

- Loads `hello.risor` from disk
- Creates a label widget with "Hello World" text
- Displays the label in the window

## Code

```js
// Simple hello world example
// Creates a label widget and sets it as the window content

require("v0.2")

let label = widget.NewLabel("Hello World")
window.SetContent(container.NewHBox(label))
```

## Running

```bash
cd examples/01-hello-world
go run main.go
```

Or with the fynerisor CLI:

```bash
fynerisor script.risor
```

## Full Source

[View full example on GitHub ↗](https://github.com/uidbz/fynerisor/tree/main/examples/01-hello-world)

---
title: Canvas Objects
type: docs
weight: 3
---

Low-level drawing primitives for custom graphics.

## Image

Displays images from URIs (file paths or URLs).

```go
// Load image from file
img := canvas.NewImageFromURI("file:///path/to/image.png")

// Update image
img.SetImageFromURI("file:///other/image.png")

// Load from current app's resources
img := canvas.NewImageFromURI(gui.CurrentBaseURL + "logo.png")
```

**Methods**:
- `SetImageFromURI(uri)`: Update the displayed image

**Supported formats**: PNG, JPEG, GIF

**Use cases**: Logos, icons, photos, diagrams

---

## Line

Draws colored lines.

```go
line := canvas.NewLine("red")
```

**Supported colors**:
- `red`
- `green`
- `blue`
- `black`
- `white`
- `yellow`
- `cyan`
- `magenta`

**Use cases**: Separators, decorative elements, simple diagrams

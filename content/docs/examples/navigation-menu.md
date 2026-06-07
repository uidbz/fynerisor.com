---
title: Navigation Menu
type: docs
weight: 2
---

Create a menu of buttons that navigate to different apps.

## Code

```go
buttons := [
    widget.NewButton("Charts", func() { gui.goto("chart.risor") }),
    widget.NewButton("Tables", func() { gui.goto("table.risor") }),
    widget.NewButton("SQL Demo", func() { gui.goto("sql.risor") }),
]

menu := container.NewVBox(buttons)
window.SetContent(menu)
```

## Key Concepts

- **Array of buttons**: Store multiple buttons in an array for easy management
- **Navigation with `gui.goto()`**: Navigate to different scripts
- **VBox container**: Arrange buttons vertically

## Use Cases

- Main menu for your application
- Navigation between different tools
- Dashboard with quick links

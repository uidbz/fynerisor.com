---
title: Hello World Tutorial
type: docs
weight: 2
prev: /
next: docs/quick-reference/
---

A step-by-step guide to creating your first Fynerisor application using Risor v2 syntax.

{{% steps %}}

### Step 1: Create Your Script

Create a new text file **helloworld.risor**:

{{< tabs items="helloworld.risor" >}}
{{< tab >}}

```js
require(["v0.4", "@gui"])

let lbl = widget.NewLabel("Hello world")
window.SetContent(lbl)
```
{{< /tab >}}
{{< /tabs >}}

**What this does**:
- `require(["v0.4", "@gui"])` - Declares version and loads the GUI module
- Creates a label widget displaying "Hello world"
- Sets the label as the main window content using `window.SetContent()`

{{< callout emoji="💡" >}}
**Risor v2 Syntax**: Note the use of `let` for variables and `require()` for modules. Fynerisor uses modern Risor v2 syntax with arrow functions and functional iteration.
{{< /callout >}}

### Step 2: Run Your Script

Run the script with the Fynerisor CLI tool:

```bash
fynerisor helloworld.risor
```

Or use watch mode for automatic reloading:

```bash
fynerisor --watch helloworld.risor
```

### Step 3: See Your App

Your app should now be running, displaying "Hello world" in the window.

{{% /steps %}}

## Live Reload with Watch Mode

{{< callout type="tip" >}}
When using `--watch` mode, any changes you make to the .risor file will automatically reload the application when you save. No manual refresh needed!
{{< /callout >}}

## Adding Interactivity

Let's make it interactive with a button:

```js
require(["v0.4", "@gui"])

let count = 0
let label = widget.NewLabel(sprintf("Count: %d", count))

let btn = widget.NewButton("Click Me", () => {
    count = count + 1
    label.SetText(sprintf("Count: %d", count))
})

window.SetContent(container.NewVBox([btn, label]))
```

**New concepts**:
- **Arrow functions**: `() => { }` for callbacks (Risor v2 syntax)
- **Containers**: `container.NewVBox()` stacks widgets vertically
- **State**: Variables updated in callbacks
- **sprintf**: Built-in function for string formatting

## Using Data Binding

For reactive UIs that automatically update, use data binding:

```js
require(["v0.4", "@gui"])

let count = binding.NewInt(0)
let label = widget.NewLabelWithData(
    count.map(x => sprintf("Count: %d", x))
)

let btn = widget.NewButton("Click Me", () => {
    count.Set(count.Get() + 1)
})

window.SetContent(container.NewVBox([btn, label]))
```

**Data binding benefits**:
- Automatic UI updates when data changes
- No manual `SetText()` calls needed
- Clean separation of data and presentation

## Next Steps

Now that you've created your first app, try:

- **More Widgets**: See [API Reference - Widgets](../api-reference/widgets)
- **Layouts**: Learn about containers in [API Reference - Containers](../api-reference/containers)
- **Data Binding**: Advanced guide in [Advanced - Data Binding](../advanced/data-binding)
- **Real Examples**: Browse the [Examples](../examples) section

Enjoy building with Fynerisor!

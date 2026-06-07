---
title: API Integration
type: docs
weight: 4
---

Fetch data from an API and display results.

## Code

```go
lbl := widget.NewLabel("CID:")
entry := widget.NewEntry()
result := widget.NewEntry()

btn := widget.NewButton("Go", func() {
    reply := fetch("http://api.example.com/cid/" + entry.Text).json()
    if reply.Success {
        text := "CID:\t\t" + string(reply.Entry["CID"]) + "\n"
        text += "SMILES:\t" + reply.Entry["SMILES"] + "\n"
        text += "InChI:\t\t" + reply.Entry["InChI"] + "\n"
        text += "InChI key:\t" + reply.Entry["InChIKey"] + "\n"
        synonyms := reply.Entry["Synonyms"] | strings.join("\n")
        text += "Synonyms:\n" + synonyms
        result.Text = text
    } else {
        result.Text = "Something went wrong: " + reply.ErrorMsg
    }
})

back := widget.NewButton("Back to index", func() { gui.goto("..") })
top := container.NewBorder(nil, nil, lbl, nil, entry)
border := container.NewBorder(container.NewVBox(top, btn), back, nil, nil, result)
window.SetContent(border)
```

## Key Concepts

- **`fetch()` for HTTP requests**: Simple API calls
- **`.json()` to parse response**: Parse JSON automatically
- **String concatenation**: Build formatted output
- **Border layout**: Complex UI with header and footer
- **Navigate up**: Use `gui.goto("..")` to go back

## Use Cases

- REST API integration
- Data lookup tools
- External service integration
- Real-time data fetching

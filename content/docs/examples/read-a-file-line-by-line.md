---
title: Read a File Line by Line
type: docs
weight: 9
---

Process text files line by line and display output in a log widget.

## Code

```go
input := widget.NewEntry()
input.Text = "C:/Temp/input.txt"
items := [
    widget.NewFormItem("Path to file", input),
]

output := widget.NewLog(100)
gui.CaptureStdout(func(text) {
    output.Append(text)
})

top := widget.NewForm(items)
border := container.NewBorder(top, nil, nil, nil, output)

data := os.read_file(input.Text)
clean := strings.replace_all(string(data), "\r\n", "\n")

parts := strings.split(clean, "\n")
for i, x := range parts {
    print(string(i) + " " + x)
}

window.SetContent(border)
```

## Key Concepts

- **File reading**: `os.read_file()` reads entire file
- **String normalization**: Handle both Windows (`\r\n`) and Unix (`\n`) line endings
- **String splitting**: `strings.split()` divides text into lines
- **Log widget**: Display output with scrolling
- **Capture stdout**: Redirect `print()` statements to log widget

## Improvements

### Interactive Version

Add a button to reload the file:

```go
btnLoad := widget.NewButton("Load File", func() {
    output.Clear()  // Clear previous content
    data := os.read_file(input.Text)
    clean := strings.replace_all(string(data), "\r\n", "\n")
    parts := strings.split(clean, "\n")
    for i, x := range parts {
        print(string(i) + " " + x)
    }
})
```

### Error Handling

Add error checking:

```go
if !os.exists(input.Text) {
    window.SetStatus("Error: File not found")
    return
}
```

## Use Cases

- Log file viewer
- Text file processor
- Data import tool
- File content inspection
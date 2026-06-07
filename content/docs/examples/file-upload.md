---
title: File Upload with Drag & Drop
type: docs
weight: 6
---

Upload files with drag-and-drop support.

## Code

```go
logo := canvas.NewImageFromURI(gui.CurrentBaseURL + "iff-logo.png")
title := widget.NewLabel("Secure upload to server")
txtPath := widget.NewEntry()
txtResult := widget.NewEntry()
txtResult.Text = "Nothing has been uploaded"

btnGo := widget.NewButton("Upload", func() {
    window.SetStatus("Uploading...")
    result := lars.Upload(txtPath.Text)
    txtResult.Text = result.Hash
    window.SetStatus("Uploading... Done!")
})

items := [
    widget.NewFormItem("File/directory to upload (drag here)", txtPath),
    widget.NewFormItem("Result hash", txtResult),
    widget.NewFormItem("", widget.NewLabel("Notice: Files are ONLY retrievable via this hash")),
    widget.NewFormItem("", btnGo),
]

form := widget.NewForm(items)
vbox := container.NewVBox(logo, title, form)

gui.OnDropped(func(files) {
    txtPath.Text = files[0]
})

window.SetContent(vbox)
```

## Key Concepts

- **`gui.OnDropped()`**: Handle drag-and-drop events
- **`gui.CurrentBaseURL`**: Load resources relative to script location
- **Status updates**: Use `window.SetStatus()` for user feedback
- **Custom API integration**: Example using lars.Upload

## Use Cases

- File upload interfaces
- Batch file processing
- Document management
- Data import tools

## Drag & Drop Tips

```go
gui.OnDropped(func(files) {
    // files is an array of paths
    for _, path := range files {
        print("Processing:", path)
        // Process each file
    }
})
```

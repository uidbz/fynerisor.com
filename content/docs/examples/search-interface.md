---
title: Advanced Search Interface
type: docs
weight: 7
---

Complex search with filters and clickable results.

## Code

```go
table := widget.NewTable("search_results", 150)
query := widget.NewEntry()
topLevelOnly := widget.NewCheck("Filter out subdirectories", func(checked) {})
topLevelOnly.SetChecked(true)

server := "http://search-api.example.com"
searchTerms := ""
resultCount := 0
data := []

// Make table rows clickable
table.SetOnClick(func(row, col) {
    gui.OpenBrowser(data[row][1])  // Open full path in browser
})

table.Columns(func() {
    return ["Filename", "Full path", "Type", "Keywords", "Score"]
})

table.SetColumnWidth(0, 500)
table.SetColumnWidth(1, 500)
table.SetColumnWidth(2, 100)

table.RowCount(func() {
    return int(resultCount)
})

table.Data(func(offset, limit) {
    page := offset/limit + 1

    queryText := server + "/search?terms=" + searchTerms +
        "&page=" + string(page) +
        "&limit=" + string(limit) +
        "&top_level_only=" + string(topLevelOnly.Checked)

    reply := fetch(queryText).json()
    resultCount = reply["total"]

    if resultCount == 0 {
        return []
    }

    data = []
    for _, x := range reply["results"] {
        data.append([
            filepath.base(x["path"]),
            x["path"],
            x["type"],
            x["matched_keywords"],
            x["match_score"],
        ])
    }

    return data
})

func populate_table() {
    window.SetStatus("Searching...")
    searchTerms = query.Text.replace_all(" ", ",")
    table.Refresh()
    window.SetStatus("Search complete!")
}

btnGo := widget.NewButton("Go", populate_table)
query.OnSubmitted(func(text) {
    populate_table()
})

// Filter by file type
typeList := ["Directories", "Files", "Excel", "Word", "PDF", "Images"]
selectType := widget.NewSelect(typeList, func(selected) {
    // Handle selection
})
selectType.SetSelectedIndex(0)

items := [
    widget.NewFormItem("Search terms", container.NewBorder(nil, nil, nil, btnGo, query)),
    widget.NewFormItem("Search for", selectType),
    widget.NewFormItem("Top level results only", topLevelOnly),
]

top := widget.NewForm(items)
border := container.NewBorder(top, nil, nil, nil, table)
window.SetContent(border)
```

## Key Concepts

- **Clickable table**: `table.SetOnClick()` handles row clicks
- **Custom column widths**: `table.SetColumnWidth()` for precise layout
- **Entry submit callback**: `entry.OnSubmitted()` triggers on Enter key
- **Select dropdown**: Type filtering with dropdown
- **Dynamic search**: API pagination with real-time results
- **State management**: Store search state in variables

## Advanced Features

### Clickable Rows

```go
table.SetOnClick(func(row, col) {
    // row and col are 0-indexed
    cellData := data[row][col]
    // Perform action based on click
})
```

### Column Width Configuration

```go
table.SetColumnWidth(0, 500)  // First column: 500px
table.SetColumnWidth(1, 300)  // Second column: 300px
```

### Submit on Enter

```go
entry.OnSubmitted(func(text) {
    // Triggered when user presses Enter
    performSearch(text)
})
```

## Use Cases

- File search tools
- Database search interfaces
- Document management systems
- Data exploration tools

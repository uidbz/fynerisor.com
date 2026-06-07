---
title: Data Table with Pagination
type: docs
weight: 3
---

Display data in a paginated table.

## Code

```go
table := widget.NewTable("mydata", 10)

data := [
    ["1", "2", "3"],
    ["7", "8", "9"],
    ["7", "80", "91"],
    ["71", "8", "1"],
    ["6", "8", "1"],
    ["7", "7", "7"],
]

table.Columns(func() {
    return ["Column1", "Column2", "Column3"]
})

table.RowCount(func() {
    return len(data)
})

table.Data(func(offset, limit) {
    if limit > len(data) {
        limit = len(data)
    }
    return data[offset:limit]
})

table.Refresh()
window.SetContent(table)
```

## Key Concepts

- **Table widget with page size**: Second parameter (10) sets rows per page
- **Callback functions**: Define columns, row count, and data separately
- **Pagination**: Use offset/limit for efficient data display
- **Array slicing**: `data[offset:limit]` syntax for pagination
- **Refresh**: Call `table.Refresh()` to display the table

## Use Cases

- Display database query results
- Show CSV file contents
- Present search results
- Data browsing and exploration

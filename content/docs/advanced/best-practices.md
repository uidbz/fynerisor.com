---
title: Best Practices
type: docs
weight: 3
---

Guidelines for building robust, maintainable Fynerisor applications.

## Code Organization

### File Structure

Organize your apps logically:

```
goto-apps/
├── index.risor           # Main menu/landing page
├── tools/
│   ├── index.risor      # Tools menu
│   ├── search.risor     # Search tool
│   └── report.risor     # Report generator
└── shared/
    ├── config.json      # Shared configuration
    └── logo.png         # Shared resources
```

### Script Comments

Document your code:

```go
// Chemical Search Tool
// Author: John Doe
// Last updated: 2026-03-15
//
// Searches the chemical database by CID, SMILES, or name
// and displays results in a paginated table.

// Initialize database connection
conn := sql.connect("sqlserver://server/db", {"stream": true})

// Configure search parameters
searchLimit := 100
```

## Performance

### Efficient Data Loading

Load data on demand, not all at once:

```go
// Good: Load data with pagination
table.Data(func(offset, limit) {
    query := "SELECT * FROM data OFFSET " + string(offset) +
             " ROWS FETCH NEXT " + string(limit) + " ROWS ONLY"
    return conn.query(query)
})

// Bad: Load all data upfront
allData := conn.query("SELECT * FROM data")  // Could be millions of rows!
```

### Lazy Initialization

Create expensive resources only when needed:

```go
conn := nil  // Start as nil

btnSearch := widget.NewButton("Search", func() {
    if conn == nil {
        window.SetStatus("Connecting to database...")
        conn = sql.connect("sqlserver://server/db", {"stream": true})
    }
    // Perform search
})
```

### Caching Results

Cache API responses when appropriate:

```go
cache := {}

func fetchData(id) {
    if cache[id] != nil {
        return cache[id]
    }

    data := fetch("https://api.example.com/data/" + id).json()
    cache[id] = data
    return data
}
```

## User Experience

### Loading Indicators

Always provide feedback for long operations:

```go
btnProcess := widget.NewButton("Process", func() {
    window.SetStatus("Processing... Please wait")

    // Long operation
    result := processLargeFile(file)

    window.SetStatus("Done! Processed " + string(result.count) + " items")
})
```

### Error Handling

Handle errors gracefully:

```go
btnLoad := widget.NewButton("Load", func() {
    if !os.exists(path.Text) {
        window.SetStatus("Error: File not found")
        return
    }

    data := os.read_file(path.Text)
    if len(data) == 0 {
        window.SetStatus("Warning: File is empty")
        return
    }

    // Process data
    window.SetStatus("File loaded successfully")
})
```

### Input Validation

Validate user input:

```go
btnSubmit := widget.NewButton("Submit", func() {
    // Trim whitespace
    username := strings.trim_space(txtUsername.Text)

    // Check required fields
    if username == "" {
        window.SetStatus("Error: Username is required")
        return
    }

    // Validate format
    if !strings.contains(username, "@") {
        window.SetStatus("Error: Invalid email format")
        return
    }

    // Submit
    submit(username)
})
```

## Security

### SQL Injection Prevention

Always escape user input for SQL:

```go
func sqlEscape(text) {
    return strings.replace_all(text, "'", "''")
}

query := "SELECT * FROM users WHERE name = '" + sqlEscape(userInput) + "'"
```

Better: Use parameterized queries if available.

### Path Traversal Prevention

Validate file paths:

```go
func isValidPath(path) {
    // Don't allow .. in paths
    if strings.contains(path, "..") {
        return false
    }

    // Ensure path is in allowed directory
    if !strings.starts_with(path, "S:\\allowed\\") {
        return false
    }

    return true
}

if !isValidPath(userPath) {
    window.SetStatus("Error: Invalid path")
    return
}
```

### Credential Protection

Never log or display credentials:

```go
// Good
window.SetStatus("Connecting to " + server + "...")

// Bad - exposes password in status bar
window.SetStatus("Connecting as " + username + ":" + password + "...")
```

## Testing

### Test with Sample Data

Create test data for development:

```go
// Development mode
devMode := true

if devMode {
    data := [
        ["Test1", "Value1"],
        ["Test2", "Value2"],
    ]
} else {
    data = fetchRealData()
}
```

### Error Simulation

Test error handling:

```go
// Simulate API failure
if devMode {
    response := {"Success": false, "ErrorMsg": "Connection timeout"}
} else {
    response = fetch("https://api.example.com/data").json()
}

if !response.Success {
    window.SetStatus("Error: " + response.ErrorMsg)
    return
}
```

## Maintainability

### Reusable Functions

Extract common logic:

```go
func createStandardTable(title, data, columns) {
    table := widget.NewTable(title, 20)
    table.Columns(func() { return columns })
    table.RowCount(func() { return len(data) })
    table.Data(func(offset, limit) {
        if limit > len(data) {
            limit = len(data)
        }
        return data[offset:limit]
    })
    table.Refresh()
    return table
}

// Use it
myTable := createStandardTable("Users", userData, ["ID", "Name", "Email"])
```

### Configuration Files

Store configuration separately:

```go
// Load config from JSON file
configText := os.read_file(gui.CurrentBaseURL + "config.json")
config := json.parse(string(configText))

apiUrl := config.apiUrl
pageSize := config.pageSize
```

### Version Information

Include version info in your apps:

```go
version := "v1.2.0"

about := widget.NewLabel("Chemical Search Tool " + version + "\nLast updated: 2026-03-15")
```

## Naming Conventions

Use clear, descriptive names:

```go
// Good
btnSubmitSearch := widget.NewButton("Search", searchHandler)
txtChemicalName := widget.NewEntry()
tblSearchResults := widget.NewTable("results", 20)

// Avoid
b1 := widget.NewButton("Go", fn)
e := widget.NewEntry()
t := widget.NewTable("x", 20)
```

## Common Patterns

### Menu with Back Button

```go
func createStandardLayout(title, content) {
    header := widget.NewLabel(title)
    btnBack := widget.NewButton("← Back", func() { gui.goto("..") })

    top := container.NewBorder(nil, nil, header, btnBack, nil)
    return container.NewBorder(top, nil, nil, nil, content)
}
```

### Form with Validation

```go
func validateForm(fields) {
    for _, field := range fields {
        if field.Text == "" {
            return "All fields are required"
        }
    }
    return ""
}

btnSubmit := widget.NewButton("Submit", func() {
    error := validateForm([name, email, phone])
    if error != "" {
        window.SetStatus("Error: " + error)
        return
    }
    // Submit
})
```

### Table with Search

```go
searchEntry := widget.NewEntry()
filteredData := []

func updateTable() {
    searchTerm := strings.to_lower(searchEntry.Text)
    filteredData = []

    for _, row := range allData {
        if searchTerm == "" || strings.contains(strings.to_lower(row[0]), searchTerm) {
            filteredData.append(row)
        }
    }

    table.Refresh()
}

searchEntry.OnSubmitted(func(text) {
    updateTable()
})
```

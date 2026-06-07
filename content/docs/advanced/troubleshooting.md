---
title: Troubleshooting
type: docs
weight: 4
---

Common issues and their solutions.

## Script Loading Issues

### Script Not Found (404)

**Symptoms**: goto shows "404 Not Found" or blank screen

**Causes**:
- Script file doesn't exist at the specified path
- Incorrect URL
- Network path not accessible

**Solutions**:
1. Verify the file exists in the local goto directory
2. Check the URL matches the file path exactly
3. Ensure network drive is mounted (Windows: check S: drive)
4. Check file permissions

```bash
# Linux: Check if network share is mounted
df -h | grep /mnt/s-drive

# Windows: Check S: drive in File Explorer
```

### Blank Screen / No Content

**Symptoms**: goto opens but shows nothing

**Causes**:
- Script doesn't call `window.SetContent()`
- Script has errors before reaching `window.SetContent()`
- Widget creation failed

**Solutions**:
1. Ensure script calls `window.SetContent()`:
```go
content := widget.NewLabel("Hello")
window.SetContent(content)  // Don't forget this!
```

2. Check for errors in script logic
3. Test with minimal script first

## Runtime Errors

### Variable Not Defined

**Error**: `undefined variable: myVar`

**Cause**: Variable used before declaration

**Solution**:
```go
// Wrong
print(myVar)
myVar := "value"

// Correct
myVar := "value"
print(myVar)
```

### Nil Pointer / Null Reference

**Error**: Accessing properties of nil object

**Cause**: Object is nil/undefined

**Solution**:
```go
// Check before use
if data != nil {
    print(data.field)
}

// Or use default
value := ""
if data != nil {
    value = data.field
}
```

### Type Errors

**Error**: Type mismatch or conversion error

**Solution**:
```go
// Convert types explicitly
numStr := "123"
num := int(numStr)  // Convert string to int

// For database IDs
id := string(row["id"])  // Convert to string
```

## Database Issues

### Connection Failed

**Symptoms**: SQL connection errors

**Solutions**:
1. Verify connection string:
```go
conn := sql.connect(
    "sqlserver://username:password@server:1433/database",
    {"stream": true}
)
```

2. Check credentials in config file
3. Verify network access to database server
4. Test with SQL Server Management Studio first

### Query Returns No Data

**Causes**:
- Incorrect SQL syntax
- No matching records
- Wrong table/column names

**Solutions**:
1. Test query in SQL client first
2. Add error handling:
```go
rows := conn.query("SELECT * FROM MyTable")
if len(rows) == 0 {
    window.SetStatus("No results found")
    return
}
```

3. Print query for debugging:
```go
query := "SELECT * FROM Table WHERE id = " + string(id)
print(query)  // Check the actual query
rows := conn.query(query)
```

## API Integration Issues

### Fetch Fails / Timeout

**Symptoms**: API requests fail or timeout

**Solutions**:
1. Check URL is correct:
```go
url := "https://api.example.com/data"
print("Fetching:", url)  // Verify URL
response := fetch(url)
```

2. Handle network errors:
```go
response := fetch(url)
if response.status != 200 {
    window.SetStatus("API Error: " + string(response.status))
    return
}
```

3. Increase timeout if needed
4. Check proxy settings if behind corporate firewall

### JSON Parse Error

**Symptoms**: `.json()` fails or returns unexpected data

**Solutions**:
1. Check response format:
```go
response := fetch(url)
print("Response:", response.text)  // See raw response
data := response.json()
```

2. Verify API returns JSON
3. Handle parse errors:
```go
if response.headers["Content-Type"] != "application/json" {
    window.SetStatus("Error: Expected JSON response")
    return
}
```

## Table Widget Issues

### Table Not Showing Data

**Causes**:
- Forgot to call `table.Refresh()`
- Data callbacks not returning correct format
- RowCount returns 0

**Solutions**:
```go
table := widget.NewTable("data", 20)

// Set callbacks
table.Columns(func() {
    return ["Col1", "Col2"]
})

table.RowCount(func() {
    count := len(data)
    print("Row count:", count)  // Debug
    return count
})

table.Data(func(offset, limit) {
    print("Fetching rows:", offset, "to", limit)  // Debug
    return data[offset:limit]
})

table.Refresh()  // DON'T FORGET THIS!
```

### Table Pagination Not Working

**Cause**: Incorrect offset/limit handling

**Solution**:
```go
table.Data(func(offset, limit) {
    // Handle edge case
    if offset >= len(data) {
        return []
    }

    end := offset + limit
    if end > len(data) {
        end = len(data)
    }

    return data[offset:end]
})
```

## Performance Issues

### Slow Loading

**Causes**:
- Loading too much data at once
- Inefficient queries
- No pagination

**Solutions**:
1. Use pagination:
```go
// Good: Server-side pagination
query := "SELECT * FROM data OFFSET " + string(offset) +
         " ROWS FETCH NEXT " + string(limit) + " ROWS ONLY"

// Bad: Load everything
query := "SELECT * FROM data"  // Could be millions of rows!
```

2. Add loading indicator:
```go
window.SetStatus("Loading...")
data := fetchData()
window.SetStatus("Ready")
```

### UI Freezing

**Cause**: Long-running operations block UI

**Solution**: Provide feedback and break up operations:
```go
window.SetStatus("Processing... (this may take a moment)")

// Process in batches
for i, item := range items {
    processItem(item)

    // Update progress
    if i % 100 == 0 {
        window.SetStatus("Processing... " + string(i) + "/" + string(len(items)))
    }
}
```

## File Operations

### File Not Found

**Solution**:
```go
path := txtPath.Text

if !os.exists(path) {
    window.SetStatus("Error: File not found: " + path)
    return
}

data := os.read_file(path)
```

### Permission Denied

**Causes**:
- No read/write permission
- File is locked by another process

**Solutions**:
1. Check file permissions
2. Close file in other applications
3. Run goto with appropriate permissions

## Cross-Platform Issues

### Path Separators

**Problem**: Hard-coded paths don't work on all platforms

**Solution**:
```go
// Use filepath.join
path := filepath.join(["data", "files", "input.txt"])

// Or check OS
if gui.GOOS == "windows" {
    path := "S:\\data\\file.txt"
} else {
    path := "/mnt/s-drive/data/file.txt"
}
```

### Line Endings

**Problem**: Windows files have `\r\n`, Unix has `\n`

**Solution**:
```go
// Normalize line endings
data := os.read_file(path)
text := string(data)
text = strings.replace_all(text, "\r\n", "\n")
lines := strings.split(text, "\n")
```

## Debugging Tips

### Add Debug Output

```go
// Print variables
print("Debug: value =", myVar)

// Print JSON for complex objects
print("Debug: data =", json.generate(data))

// Use status bar
window.SetStatus("Debug checkpoint 1")
```

### Simplify to Isolate Issue

Start with minimal working code:

```go
// Step 1: Just show content
window.SetContent(widget.NewLabel("Test"))

// Step 2: Add one feature
// Step 3: Add another feature
// ...until you find what breaks
```

### Check Browser Console

For web-related issues, check browser console (F12) when using `gui.OpenBrowser()`.

## Getting Help

If you're still stuck:

1. Check this documentation
2. Review examples for similar functionality
3. Test with simpler code to isolate the issue
4. Contact the goto development team
5. Check GitLab issues: https://brabgit.global.iff.com/brablab/goto/issues

## Common Error Messages

| Error | Meaning | Solution |
|-------|---------|----------|
| `undefined variable` | Variable not declared | Declare variable before use |
| `type error` | Wrong data type | Convert types explicitly |
| `nil pointer` | Accessing nil object | Check for nil before access |
| `index out of range` | Array index invalid | Check array length |
| `connection refused` | Can't reach server | Check network/credentials |
| `syntax error` | Invalid Risor syntax | Check script syntax |

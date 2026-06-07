---
title: Global Objects
type: docs
weight: 5
---

Fynerisor provides several global objects for specialized functionality including data binding, modules, and constants.

All examples use **Risor v2 syntax** with arrow functions.

## window

Main window control object (GUI mode only).

```js
// Set window content
window.SetContent(widget)

// Set status message
window.SetStatus("Processing...")

// Get or set window title
window.Title = "My App"
let currentTitle = window.Title

// Queue GUI update from background thread
window.Do(() => {
    label.SetText("Updated from background")
})

// Handle file drops
window.OnDropped((paths) => {
    paths.each(path => print("Dropped:", path))
})

// Access dropped file paths
let paths = window.DroppedPaths
```

**Methods**:
- `window.SetContent(widget)` - Set main window content
- `window.SetStatus(text)` - Update status message
- `window.Do(callback)` - Execute callback on UI thread (for goroutine safety)
- `window.OnDropped(callback)` - Handle file drop events

**Properties**:
- `window.Title` - Get/set window title (read/write)
- `window.DroppedPaths` - Array of dropped file paths

**Use cases**: Window management, status updates, file drop handling, thread-safe GUI updates

---

## binding

Data binding system for reactive UIs. Automatically updates widgets when data changes.

### String Binding

```js
let name = binding.NewString("John")
let label = widget.NewLabelWithData(name)

// Update triggers automatic UI refresh
name.Set("Jane")

// Read current value
let current = name.Get()
```

### Bool Binding

```js
let enabled = binding.NewBool(false)
let check = widget.NewCheckWithData("Enable", enabled)

// Listen for changes
enabled.AddListener(() => {
    print("Enabled:", enabled.Get())
})
```

### Int Binding

```js
let count = binding.NewInt(0)
let label = widget.NewLabelWithData(
    count.map(x => sprintf("Count: %d", x))
)

// Increment
count.Set(count.Get() + 1)
```

### Float Binding

```js
let value = binding.NewFloat(50.0)
let slider = widget.NewSliderWithData(0, 100, value)
let label = widget.NewLabelWithData(
    value.map(x => sprintf("Value: %.1f", x))
)
```

**Binding Methods**:
- `.Get()` - Retrieve current value
- `.Set(value)` - Update value (triggers UI updates)
- `.AddListener(callback)` - React to changes
- `.map(fn)` - Transform binding value for display

**Use cases**: Reactive UIs, two-way data binding, automatic UI updates

**See also**: [Data Binding Examples](../../examples/data-binding)

---

## constants

Fyne constants for styling and behavior.

```js
// Button/Label importance
button.Importance = constants.ImportanceHigh
button.Importance = constants.ImportanceMedium
button.Importance = constants.ImportanceLow
button.Importance = constants.DangerImportance
button.Importance = constants.WarningImportance
button.Importance = constants.SuccessImportance

// Text wrapping
label.Wrapping = constants.TextWrapOff
label.Wrapping = constants.TextWrapBreak
label.Wrapping = constants.TextWrapWord

// Text truncation
label.Truncation = constants.TextTruncateOff
label.Truncation = constants.TextTruncateClip
label.Truncation = constants.TextTruncateEllipsis

// Text alignment
label.Alignment = constants.TextAlignLeading
label.Alignment = constants.TextAlignCenter
label.Alignment = constants.TextAlignTrailing
```

**Use cases**: Widget styling, text formatting, visual hierarchy

---

## app

Application metadata.

```js
// Access application name (set via WithAppName)
let appName = app.name

// Access application version (set via SetAppVersion)
let version = app.version
```

**Properties**:
- `app.name` - Application name
- `app.version` - Application version

**Use cases**: Display app info, version checking

---

## Risor v2 Standard Modules

Fynerisor includes standard Risor v2 modules. Load with `require()`:

```js
require(["@http", "@sql", "@os", "@time"])
```

### HTTP Module (`@http`)

HTTP client for API requests.

```js
require("@http")

// GET request
let response = http.get("https://api.example.com/data")
print(response.status)
print(response.body)
let data = response.json()

// POST request
let payload = {"key": "value"}
let response = http.post("https://api.example.com/submit", 
    {"Content-Type": "application/json"},
    payload)
```

**Use cases**: REST API calls, web services, data fetching

---

### SQL Module (`@sql`)

SQL database connectivity.

```js
require("@sql")

// Connect to database
let conn = sql.connect("sqlite3::memory:")

// Query data (streaming iterator)
let rows = conn.query("SELECT * FROM Users WHERE active = ?", 1)

rows.each(row => {
    print(sprintf("%s: %s", row["username"], row["email"]))
})

// Or collect all first
let allRows = conn.query("SELECT * FROM users").collect()

// Execute statement
conn.exec("INSERT INTO Logs (message) VALUES (?)", "App started")

// Close connection
conn.close()
```

**Supported databases**: SQL Server, MySQL, PostgreSQL, SQLite

**Connection strings**:
- SQL Server: `sqlserver://user:pass@server:1433/database`
- PostgreSQL: `postgres://user:pass@server:5432/database`
- MySQL: `mysql://user:pass@server:3306/database`
- SQLite: `sqlite3://./app.db` or `sqlite3::memory:`

**Use cases**: Database queries, data storage, CRUD operations

---

### OS Module (`@os`)

Operating system and file system operations.

```js
require("@os")

// Platform detection
let platform = os.goos()  // "linux", "windows", "darwin"

// Current user
let user = os.current_user()

// Open browser
os.open_browser("https://example.com")

// File operations
os.write_file("output.txt", "Hello, World!")
let data = os.read_file("input.txt")
let text = string(data)

// Directory operations
os.read_dir("/path/to/dir").each(entry => {
    print(entry.name, entry.is_dir, entry.size)
})
```

**Use cases**: Platform detection, file I/O, system operations

---

### File I/O Module (`@io`)

File operations.

```js
require("@io")

// Copy file
io.cp("source.txt", "destination.txt")

// Read file as string
let content = io.read_all("/path/to/file.txt")
```

**Use cases**: File copying, reading

---

### Time Module (`@time`)

Date and time handling.

```js
require("@time")

// Current time
let now = time.now()

// Create date
let date = time.date(2026, 5, 21)

// Parse date string
let parsed = time.parse("2026-05-21")

// Format date
let formatted = now.format("2006-01-02 15:04:05")
```

**Use cases**: Date/time operations, formatting, parsing

---

### Strings Module (`@strings`)

String manipulation utilities.

```js
require("@strings")

// Convert case
let upper = strings.upper("hello")
let lower = strings.lower("HELLO")

// Contains/prefix/suffix
let contains = strings.contains("hello world", "world")
let hasPrefix = strings.has_prefix("hello", "he")

// Split/join
let parts = strings.split("a,b,c", ",")
let joined = strings.join(["a", "b", "c"], ",")

// Trim
let trimmed = strings.trim("  hello  ")
```

**Use cases**: String processing, text manipulation

---

### Filepath Module (`@filepath`)

File path operations.

```js
require("@filepath")

// Join paths
let fullPath = filepath.join("/home", "user", "file.txt")

// Get directory
let dir = filepath.dir("/home/user/file.txt")  // "/home/user"

// Get filename
let base = filepath.base("/home/user/file.txt")  // "file.txt"

// Get extension
let ext = filepath.ext("file.txt")  // ".txt"
```

**Use cases**: Path manipulation, file name parsing

---

## Complete Example: Database Table Viewer

```js
require(["v0.4", "@gui", "@sql"])

// Connect to database
let conn = sql.connect("sqlite3::memory:")
conn.exec("CREATE TABLE users (id INT, name TEXT, email TEXT)")
conn.exec("INSERT INTO users VALUES (1, 'Alice', 'alice@example.com')")
conn.exec("INSERT INTO users VALUES (2, 'Bob', 'bob@example.com')")

// Create table widget
let table = widget.NewTable("Users", 20)

table.Columns(() => ["ID", "Name", "Email"])

table.RowCount(() => {
    let result = conn.query("SELECT COUNT(*) as count FROM users").collect()
    return result[0]["count"]
})

table.Data((offset, limit) => {
    let query = sprintf("SELECT * FROM users LIMIT %d OFFSET %d", limit, offset)
    return conn.query(query).map(row => [
        string(row["id"]),
        row["name"],
        row["email"]
    ])
})

table.SetColumnWidth(0, 60)
table.SetColumnWidth(1, 150)
table.SetColumnWidth(2, 200)

table.Refresh()
window.SetContent(table)
```

## Summary

Fynerisor provides these global objects:

- **window** - Window control and GUI updates
- **app** - Application metadata
- **binding** - Data binding for reactive UIs (String, Bool, Int, Float)
- **constants** - Fyne styling constants
- **widget** - Widget factory functions
- **container** - Container factory functions
- **canvas** - Canvas object factory
- **chart** - Chart factory (experimental)

**Risor v2 Modules** (via `require()`):
- **@gui** - GUI functionality (automatic in gui mode)
- **@http** - HTTP client
- **@sql** - Database connectivity
- **@os** - OS operations and file I/O
- **@io** - File I/O utilities
- **@time** - Date/time handling
- **@strings** - String utilities
- **@filepath** - Path operations

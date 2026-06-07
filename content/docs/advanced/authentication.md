---
title: Authentication
type: docs
weight: 2
---

Implementing authentication in Fynerisor applications.

## HTTP Basic Auth

goto has built-in support for HTTP Basic Authentication when fetching scripts.

### Automatic Authentication

When goto encounters a 401 Unauthorized response:

1. It checks for stored credentials matching the domain
2. Automatically retries the request with credentials
3. Displays the content if authentication succeeds

### Storing Credentials

Store credentials in the [configuration file](./configuration):

```toml
[credentials."secure.example.com:latest"]
id = "secure.example.com:latest"
username = "myuser"
password = "mypass"
```

### Using in Scripts

For API calls within your scripts:

```go
creds := config.Credentials["api.example.com:latest"]

response := fetch("https://api.example.com/data", {
    "headers": {
        "Authorization": "Basic " + base64.encode(creds.Username + ":" + creds.Password)
    }
})
```

## Custom Authentication

### Bearer Token Auth

```go
token := "your-api-token"

response := fetch("https://api.example.com/data", {
    "headers": {
        "Authorization": "Bearer " + token
    }
})
```

### API Key Auth

```go
apiKey := "your-api-key"

response := fetch("https://api.example.com/data", {
    "headers": {
        "X-API-Key": apiKey
    }
})
```

## Login Forms

Create a login form in your Fynerisor app:

```go
username := widget.NewEntry()
password := widget.NewEntry()
password.Password = true  // Hide password input

btnLogin := widget.NewButton("Login", func() {
    response := fetch("https://api.example.com/login", {
        "method": "POST",
        "headers": {"Content-Type": "application/json"},
        "body": '{"username":"' + username.Text + '","password":"' + password.Text + '"}'
    })

    result := response.json()
    if result.success {
        // Store token for subsequent requests
        token := result.token
        window.SetStatus("Login successful")
        // Navigate to main app
        gui.goto("./main.risor?token=" + token)
    } else {
        window.SetStatus("Login failed: " + result.message)
    }
})

form := widget.NewForm(
    widget.NewFormItem("Username", username),
    widget.NewFormItem("Password", password),
    widget.NewFormItem("", btnLogin)
)

window.SetContent(form)
```

## Session Management

### Using URL Parameters

Pass session data via URL:

```go
// In login script
token := "abc123"
gui.goto("./dashboard.risor?token=" + token)

// In dashboard script
// Note: Risor doesn't have built-in URL parsing yet
// Consider using custom parsing or global variables
```

### Using Global Variables

Share data between navigation:

```go
// Not directly supported - each script runs independently
// Use URL parameters or re-fetch with stored credentials
```

## Security Best Practices

{{< callout type="warning" >}}
**Important**: Always use HTTPS for authenticated requests to protect credentials in transit.
{{< /callout >}}

1. **Never hardcode credentials** in scripts
2. **Use HTTPS** for all authenticated endpoints
3. **Store tokens securely** using config object
4. **Implement timeout** for sessions
5. **Validate responses** before trusting data
6. **Handle auth failures** gracefully

## Example: Authenticated API Client

```go
// Get credentials from config
creds := config.Credentials["api.internal.com"]

if creds == nil {
    window.SetStatus("Error: Credentials not configured")
    return
}

// Login to get token
loginResponse := fetch("https://api.internal.com/login", {
    "method": "POST",
    "headers": {"Content-Type": "application/json"},
    "body": '{"username":"' + creds.Username + '","password":"' + creds.Password + '"}'
})

loginData := loginResponse.json()
token := loginData.token

// Use token for API requests
dataResponse := fetch("https://api.internal.com/data", {
    "headers": {
        "Authorization": "Bearer " + token
    }
})

data := dataResponse.json()

// Display data
table := widget.NewTable("data", 20)
// ... populate table with data
window.SetContent(table)
```

## Troubleshooting Authentication

### 401 Unauthorized
- Verify credentials are correct
- Check credential ID matches domain
- Ensure configuration file is properly formatted

### 403 Forbidden
- User may not have permission
- Check API endpoint access rules
- Verify token/key is valid

### CORS Issues
- Server may need CORS configuration
- Use server-side proxy if needed
- Check browser console for CORS errors

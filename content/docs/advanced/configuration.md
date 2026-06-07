---
title: Configuration
type: docs
weight: 1
---

goto uses a TOML configuration file for storing credentials and settings.

## Configuration File Location

The configuration file is stored at:

- **Linux**: `~/.config/goto/goto-config.toml`
- **Windows**: `%APPDATA%/goto/goto-config.toml`
- **macOS**: `~/Library/Application Support/goto/goto-config.toml`

## File Format

The configuration file uses TOML format:

```toml
[credentials]
[credentials."example.com:latest"]
id = "example.com:latest"
username = "user@example.com"
password = "password"

[credentials."api.internal.com"]
id = "api.internal.com"
username = "apiuser"
password = "secret123"
```

## Accessing Credentials in Scripts

Use the global `config` object to access stored credentials:

```go
// Access specific credential
creds := config.Credentials["example.com:latest"]
username := creds.Username
password := creds.Password

// Use in API calls
auth := username + ":" + password
headers := {
    "Authorization": "Basic " + base64.encode(auth)
}

response := fetch("https://example.com/api", {
    "headers": headers
})
```

## Adding Credentials

### Method 1: Manual Editing

1. Locate your configuration file
2. Open in a text editor
3. Add a new credential entry:

```toml
[credentials."myservice.com"]
id = "myservice.com"
username = "myusername"
password = "mypassword"
```

4. Save the file
5. Restart goto

### Method 2: Programmatic (Advanced)

In a goto script (requires write permissions):

```go
// This is typically not recommended
// Manual editing is preferred
```

## Security Considerations

{{< callout type="warning" >}}
**Security Note**: Credentials are stored in plain text in the configuration file. Only store credentials for internal, trusted services.
{{< /callout >}}

- Limit file permissions (read/write for owner only)
- Don't share your configuration file
- Don't commit configuration files to version control
- Use service accounts when possible
- Rotate passwords regularly

## Environment-Specific Configuration

For different environments (dev, test, prod), use different credential IDs:

```toml
[credentials."api.internal.com:dev"]
id = "api.internal.com:dev"
username = "devuser"
password = "devpass"

[credentials."api.internal.com:prod"]
id = "api.internal.com:prod"
username = "produser"
password = "prodpass"
```

Access in script:

```go
env := "prod"  // or "dev"
credId := "api.internal.com:" + env
creds := config.Credentials[credId]
```

## Troubleshooting

### Credentials Not Found

If `config.Credentials["myid"]` returns nil:

1. Check the credential ID matches exactly (case-sensitive)
2. Verify the configuration file exists
3. Ensure TOML syntax is correct
4. Restart goto after editing

### File Permissions

On Linux/macOS, ensure proper permissions:

```bash
chmod 600 ~/.config/goto/goto-config.toml
```

This restricts access to the file owner only.

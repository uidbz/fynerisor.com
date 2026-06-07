---
title: Download
type: about
---

## Install Fynerisor

### As a CLI tool

```bash
go install github.com/uidbz/fynerisor/cmd/fynerisor@latest
```

### As a library

For GUI applications:
```bash
go get github.com/uidbz/fynerisor/gui
```

For headless/static compilation:
```bash
go get github.com/uidbz/fynerisor/core
```

## Prerequisites

For GUI applications, ensure you have the Fyne prerequisites for your platform:
- [Fyne Getting Started Guide](https://docs.fyne.io/started/quick/) - Install required system dependencies

For headless (core) usage, no GUI dependencies are required.

## Run Examples

```bash
# Clone the repository
git clone https://github.com/uidbz/fynerisor
cd fynerisor/examples/01-hello-world

# Run the example
fynerisor script.risor
```

## Requirements

- Go 1.21+
- Fyne v2.7+ (for GUI applications)
- Risor v2.1+

---
title: About
type: about
---

## What is Fynerisor?

Fynerisor provides Risor v2 script bindings for the Fyne GUI toolkit, enabling you to build cross-platform desktop applications using a simple, modern scripting language.

{{< callout type="info" >}}
Write GUI applications in scripts instead of compiled code - perfect for rapid prototyping, tools, and embeddable interfaces
{{< /callout >}}

## Current Version: **v0.5.0**

Fynerisor is production-ready with comprehensive widget coverage:
- **34+ Widgets** - 60% of Fyne coverage, all high/medium priority widgets complete
- **Risor v2** - Modern syntax with arrow functions and functional iteration
- **Data Binding** - Reactive UIs with automatic updates
- **Complete Module System** - HTTP, SQL, OS, Time, File I/O, Strings, Filepath
- **Static Compilation** - Headless mode via `core` package with no GUI dependencies
- **Embeddable** - Use as a library in your Go applications

## Package Structure

Fynerisor is split into two packages:

- **`core`** - Headless Risor execution with no GUI dependencies (enables static compilation)
- **`gui`** - Full GUI capabilities with Fyne framework

This separation allows you to build CLI tools, batch processors, and server-side scripts without GUI dependencies, or full desktop applications when needed.

## Built on Modern Technologies

* **[Risor v2](https://risor.io)** - A modern scripting language with familiar syntax and arrow functions
* **[Fyne](https://fyne.io)** - A cross-platform GUI toolkit for desktop, mobile, and web
* **[Go](https://go.dev)** - Fast, reliable, and efficient compiled language

## Motivation

The project originated from the need to make computational tools accessible to non-technical users without the complexity of web development. By combining Risor's simplicity with Fyne's native widgets, Fynerisor enables scientists, engineers, and developers to quickly build and share desktop GUIs.

Key advantages:
- Simpler than web development
- No browser required
- Native desktop widgets
- Easy to embed in Go applications
- Scriptable and extensible

## AI Disclaimer

Almost everything in this library is made with AI assistance. The initial code was human-written, but has been extensively refactored and improved through AI collaboration. While this accelerates development, all code has been reviewed for correctness and quality.

## License

BSD 3-Clause License

Copyright (c) 2026, Johan Straarup (j@uid.bz) 

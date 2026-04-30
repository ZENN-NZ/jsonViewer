# JSON → Table Converter

A self-contained, offline-first tool that transforms JSON data into a readable,
interactive table — no installs, no dependencies, no network calls required.

![HTML](https://img.shields.io/badge/HTML-Single%20File-orange)
![Security](https://img.shields.io/badge/Security-OWASP%20Compliant-green)
![Offline](https://img.shields.io/badge/Mode-Offline%20Only-blue)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## What it does

Paste any JSON into the left panel and instantly see it rendered as a clean,
structured table on the right. Nested objects and arrays can be drilled into
one level at a time using the built-in expand system, with full breadcrumb
navigation to trace and retrace your path.

---

## Features

### Core
- **Split-panel layout** — editor on the left, table output on the right
- **Drag-resizable** divider between panels
- **Live JSON validation** with colour-coded status indicator
- **Format / pretty-print** button to clean up raw JSON

### Table Output
- `Array of objects` → full headed table with row numbers
- `Single object` → Key / Value table
- `Array of primitives` → Index / Value table
- **Colour-coded value types** — null, boolean, number, string, object each styled distinctly

### Nested Expansion
- Every object or array cell shows an **⊕ Expand** button with type and item count
- Clicking drills one level deeper and re-renders the table for that value
- **Breadcrumb navigation bar** appears at the top of the output panel
- Click any breadcrumb to jump back to that level instantly
- Current drill depth shown in the footer stats
- A new conversion or Clear always resets back to root

### Export
- **Copy as CSV** — exports the current table view to clipboard
- **Copy as HTML** — exports a self-contained, styled HTML table (expand buttons stripped)

### Keyboard Shortcuts
| Shortcut | Action |
|---|---|
| `Ctrl` + `Enter` | Convert JSON to table |
| `Ctrl` + `Shift` + `F` | Format / pretty-print JSON |
| `Tab` (in editor) | Insert 2 spaces |
| `←` / `→` (on divider) | Resize panels by keyboard |

---

## Security

This tool was built with security as a first-class concern.

| Control | Detail |
|---|---|
| **No external resources** | Zero CDN calls — fully offline, nothing leaves your machine |
| **XSS prevention** | All user data written via `textContent` exclusively — `innerHTML` is never used with user data |
| **Content Security Policy** | `default-src 'none'` enforced via meta tag |
| **Input size cap** | Hard 5 MB limit prevents memory exhaustion |
| **Nesting depth cap** | Max 20 levels, checked iteratively (not recursively) to prevent stack overflow |
| **Row / column caps** | 10,000 rows · 200 columns prevents DOM flooding |
| **No `eval()`** | JSON parsed only via the native `JSON.parse()` |
| **Strict mode** | `'use strict'` throughout the entire script |
| **Referrer policy** | `no-referrer` meta tag applied |

### If hosting on a web server (e.g. behind Cloudflare)

The following HTTP response headers **must be added at the server or CDN level**
as they cannot be set via a meta tag:
- X-Content-Type-Options: nosniff
- X-Frame-Options: DENY
- Strict-Transport-Security: max-age=31536000; includeSubDomains
- Permissions-Policy: geolocation=(), camera=(), microphone=()
  
> **Note:** The CSP uses `unsafe-inline` for scripts and styles. This is a known
> trade-off of the single-file, no-build-pipeline approach. The core XSS risk
> surface remains fully mitigated by the exclusive use of `textContent`.

---

## Usage

1. Download `index.html`
2. Open it in any modern browser — no server required
3. Paste your JSON into the left panel
4. Click **Convert** or press `Ctrl + Enter`
5. Use **⊕ Expand** buttons to drill into nested data
6. Use the breadcrumb bar to navigate back up
7. Export the current view via **CSV** or **HTML**

---

## Supported JSON Shapes

```json
// Array of objects → full multi-column table
[
  { "id": 1, "name": "Alice", "role": "Admin" },
  { "id": 2, "name": "Bob",   "role": "User"  }
]

// Single object → Key / Value table
{
  "version": "1.0.0",
  "author": "example",
  "active": true
}

// Array of primitives → Index / Value table
["apple", "banana", "cherry"]

// Deeply nested → use Expand buttons to drill down
{
  "user": {
    "profile": {
      "address": { "city": "London" }
    }
  }
}

# JSON Viewer
A self-contained, offline-first tool that transforms JSON data into an interactive table or tree view — no installs, no dependencies, no network calls required.

![HTML](https://img.shields.io/badge/HTML-Offline-blue) ![License](https://img.shields.io/badge/License-MIT-green)

---

## What it does
Paste any JSON into the left panel and instantly see it rendered as a clean, structured table or collapsible tree on the right. Nested objects and arrays can be drilled into using the built-in expand system, with full breadcrumb navigation to trace and retrace your path. A built-in stats panel provides a structural analysis of your JSON at a glance.

---

## Features

### Core
- Split-panel layout — editor on the left, output on the right, both in a unified dark theme
- Drag-resizable divider between panels (keyboard accessible)
- Live JSON validation with colour-coded status indicator
- Format / pretty-print button to clean up raw JSON

### Table View
- Array of objects → full headed table with row numbers
- Single object → Key / Value table
- Array of primitives → Index / Value table
- Colour-coded value types — null, boolean, number, string, object each styled distinctly

### Tree View *(new)*
- Toggle between **Table** and **Tree** output modes from the output toolbar
- Fully collapsible, hierarchical JSON tree
- Nodes are built lazily on first expand for performance
- Inline JSON preview shown while a node is collapsed
- **Expand All** / **Collapse All** toolbar controls
- Capped at 50,000 nodes to prevent DOM flooding

### Nested Expansion (Table mode)
- Every object or array cell shows an ⊕ Expand button with type and item count
- Clicking drills one level deeper and re-renders the table for that value
- Breadcrumb navigation bar appears at the top of the output panel
- Click any breadcrumb to jump back to that level instantly
- Current drill depth shown in the footer stats
- A new conversion or Clear always resets back to root

### Stats Panel *(new)*
Click the **⧠ Stats** button to open a structural analysis modal showing:
- **Top metrics** — total nodes, total keys, max depth, object count, array count, largest array size
- **Value Types** — animated bar chart showing the distribution of objects, arrays, strings, numbers, booleans, and nulls
- **Top Keys** — the 15 most frequently occurring keys across the entire JSON tree

### Export
- Copy as CSV — exports the current table view to clipboard
- Copy as HTML — exports a self-contained, styled HTML table (expand buttons stripped)

### Keyboard Shortcuts
| Shortcut | Action |
|---|---|
| `Ctrl + Enter` | Convert JSON to table |
| `Ctrl + Shift + F` | Format / pretty-print JSON |
| `Tab` (in editor) | Insert 2 spaces |
| `← / →` (on divider) | Resize panels by keyboard |
| `Escape` | Close stats panel |

---

## Security
This tool was built with security as a first-class concern.

| Control | Detail |
|---|---|
| No external resources | Zero CDN calls — fully offline, nothing leaves your machine |
| XSS prevention | All user data written via `textContent` exclusively — `innerHTML` is never used with user data |
| Content Security Policy | `default-src 'none'` enforced via meta tag |
| Input size cap | Hard 5 MB limit prevents memory exhaustion |
| Nesting depth cap | Max 20 levels, checked iteratively (not recursively) to prevent stack overflow |
| Row / column caps | 10,000 rows · 200 columns prevents DOM flooding |
| Tree node cap | 50,000 nodes maximum in tree view |
| No `eval()` | JSON parsed only via the native `JSON.parse()` |
| Strict mode | `'use strict'` throughout the entire script |
| Referrer policy | `no-referrer` meta tag applied |

### If hosting on a web server (e.g. behind Cloudflare)
The following HTTP response headers must be added at the server or CDN level as they cannot be set via a meta tag:
- X-Content-Type-Options: nosniff
- X-Frame-Options: DENY
- Strict-Transport-Security: max-age=31536000; includeSubDomains
- Permissions-Policy: geolocation=(), camera=(), microphone=()

> **Note:** The CSP uses `unsafe-inline` for scripts and styles. This is a known trade-off of the single-file, no-build-pipeline approach. The core XSS risk surface remains fully mitigated by the exclusive use of `textContent`.

---

## Usage
1. Download `jsonViewer.html`
2. Open it in any modern browser
3. Paste your JSON into the left panel
4. Click **Convert** or press `Ctrl + Enter`
5. Toggle between **Table** and **Tree** views using the output toolbar
6. Use ⊕ **Expand** buttons (table) or ▶ toggles (tree) to explore nested data
7. Click **⧠ Stats** for a structural breakdown of your JSON
8. Use the breadcrumb bar (table mode) to navigate back up through nested levels
9. Export the current view via **CSV** or **HTML**

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

// Deeply nested → use Expand buttons (table) or tree toggles (tree view)
{
  "user": {
    "profile": {
      "address": { "city": "London" }
    }
  }
}


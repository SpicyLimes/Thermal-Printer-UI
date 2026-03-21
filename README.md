# D30 Label Designer

A browser-based label designer for the **Phomemo D30** thermal label printer, built for Linux. Connects to the printer via Bluetooth (Web Bluetooth API) — no drivers or native app required.

**Live app:** https://spicylimes.github.io/Thermal-Printer-UI/

## Requirements

- A Chromium-based browser (Chrome, Chromium, Edge) — Web Bluetooth is not supported in Firefox or Safari
- HTTPS or `localhost` context (the live GitHub Pages URL satisfies this)
- Phomemo D30 (or D35, D50, Q30, Q30S) printer with Bluetooth enabled

## Quick Start

1. Open the [live app](https://spicylimes.github.io/Thermal-Printer-UI/) in Chromium
2. Turn on your D30 and make sure Bluetooth is enabled on your machine
3. Click **Connect** and select your printer from the device picker
4. Design your label, then click **Print**

## Running Locally

Web Bluetooth requires a secure context. The easiest way to run locally:

```bash
cd /path/to/repo
python3 -m http.server 8080
```

Then open `http://localhost:8080` in Chromium.

## Features

- **Elements** — Text, images, barcodes (Code128/EAN/UPC), QR codes, shapes
- **Editing** — Drag/resize/rotate, multi-select, group/ungroup, undo/redo, Center H/V
- **Label sizes** — D30-compatible presets (12mm, 14mm, 15mm tape); default 40×15mm
- **Templates** — Variable fields (`{{Name}}`) with CSV batch printing
- **Instant expressions** — Dynamic values like `[[date]]`, `[[time]]` resolved at print time
- **Export** — Save/load designs as JSON, export PNG or PDF
- **Print preview** — Dither preview shows exact thermal output before printing
- **Mobile UI** — Full touch support on screens < 768px

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+Z` | Undo |
| `Ctrl+Shift+Z` | Redo |
| `Ctrl+D` | Duplicate selected |
| `Ctrl+G` | Group selected |
| `Ctrl+Shift+G` | Ungroup |
| `Delete` | Delete selected |
| `Arrow keys` | Nudge 1px |
| `Shift+Arrow` | Nudge 10px |
| `Shift+Click` | Add to selection |

## Project Structure

```
├── index.html      # Main UI
├── app.js          # Application logic
├── canvas.js       # Canvas rendering & raster conversion
├── elements.js     # Element management
├── handles.js      # Selection handles
├── ble.js          # Web Bluetooth transport
├── printer.js      # D-series print protocol
├── constants.js    # Shared constants & label sizes
├── storage.js      # localStorage persistence
├── templates.js    # Variable/expression substitution
└── utils/
    ├── bindings.js     # Event binding helpers
    ├── errors.js       # Error handling
    └── validation.js   # Input validation
```

## Based On

This project is a modified version of **Phomymo** by [transcriptionstream](https://github.com/transcriptionstream/phomymo), narrowed to D-series printers only with a dark UI and Linux-focused workflow. Used with acknowledgement.

<details>
<summary>Original Phomymo README</summary>

# Phomymo

A free, browser-based label designer for Phomemo thermal printers. No drivers needed - connects via Bluetooth or USB.

**Try it now: https://phomymo.affordablemagic.net**

Supports Phomemo tape printers (P12, P12 Pro, A30), M02-series (M02, M02S, M02X, M02 Pro), M-series (M03, M04S, M110, M120, M200, M220, M221, M250, M260, T02), D-series (D30, D35, D50, D110, Q30, Q30S), and PM-241 (4-inch shipping labels) thermal printers.

## Features

### Elements
- **Text** - Multiple fonts (including system fonts via Local Font Access API), sizes, bold, italic, underline, horizontal and vertical alignment, background colors
- **Images** - Import with scale control and aspect ratio lock
- **Barcodes** - Code128, EAN-13, UPC-A, Code39 formats
- **QR Codes** - Automatic sizing and encoding
- **Shapes** - Rectangle, ellipse, triangle, and line with fill options

### Shape Fills
- Solid black/white fills
- 9 dithered grayscale levels (6%, 12%, 25%, 37%, 50%, 62%, 75%, 87%, 94%)
- Stroke options with adjustable width
- Rounded corners for rectangles

### Editing
- **Visual editing** - Drag to move, resize handles on corners/edges, rotation handle
- **Multi-select** - Shift+click to select multiple elements
- **Grouping** - Group elements together (Ctrl/Cmd+G), ungroup (Ctrl/Cmd+Shift+G)
- **Undo/Redo** - Full history support (Ctrl/Cmd+Z to undo, Ctrl/Cmd+Shift+Z to redo)
- **Keyboard shortcuts** - Arrow keys to nudge, Delete to remove, Ctrl/Cmd+D to duplicate
- **Layer ordering** - Raise/lower elements in z-order

### Mobile Interface
Full-featured mobile UI with touch support (activates automatically on screens < 768px):
- **Touch gestures** - Pinch to zoom, two-finger drag to pan, double-tap to edit
- **Fixed toolbar** - Quick access to add elements (Text, Image, Rect, Circle, Line, Barcode, QR)
- **Selection actions** - Edit, Copy, Forward, Back, Delete buttons when element selected
- **Slide-up properties panel** - Full property editing for all element types
- **Hamburger menu** - Access to label size, custom dimensions, connection, save/load, undo/redo, print settings
- **Feature parity** - All desktop features available on mobile

### Templates & Batch Printing
- **Variable fields** - Use `{{FieldName}}` syntax in text, barcodes, and QR codes
- **CSV import** - Load data from spreadsheet exports
- **Manual data entry** - Add/edit records in a table interface
- **Preview grid** - See all labels before printing with click-to-enlarge
- **Batch printing** - Print multiple labels with progress indicator and cancel support

### Label Sizes
- **D-series presets**: 40x12, 30x12, 22x12, 12x12, 30x14, 22x14, 40x15, 30x15
- **Round labels**: 14mm (D-series)
- Custom dimensions with live preview

### File Operations
- **Save/Load** - Persist designs to browser localStorage
- **Export/Import** - Share designs as JSON files
- **Export to PDF** - Download label as PDF with exact dimensions
- **Export to PNG** - Download label as high-resolution PNG image
- **Print settings** - Density control, multiple copies, feed adjustment

### Instant Expressions
Dynamic values using `[[expression]]` syntax:
- **Date/Time**: `[[date]]`, `[[time]]`, `[[datetime]]`, `[[timestamp]]`
- **Components**: `[[year]]`, `[[month]]`, `[[day]]`, `[[hour]]`, `[[minute]]`, `[[second]]`
- **Custom formats**: `[[date|MM/DD/YYYY]]`, `[[time|hh:mm A]]`

## Browser Requirements

- Chrome, Edge, or another Chromium-based browser
- Web Bluetooth API support (not available in Firefox or Safari)
- HTTPS or localhost

## How It Works

1. **Label Size Configuration** - Sets pixel dimensions based on label size (203 DPI)
2. **Image Processing** - Renders canvas to 1-bit monochrome raster
3. **Printing** - Sends ESC/POS commands and raster data to the printer via Web Bluetooth

## Acknowledgments

Thanks to these projects for protocol research and inspiration:

- [vivier/phomemo-tools](https://github.com/vivier/phomemo-tools) - CUPS driver with reverse-engineered protocol
- [yaddran/thermal-print](https://github.com/yaddran/thermal-print) - Printer status query commands

Libraries used:
- [JsBarcode](https://github.com/lindell/JsBarcode) - Barcode generation
- [QRCode.js](https://github.com/davidshimjs/qrcodejs) - QR code generation
- [jsPDF](https://github.com/parallax/jsPDF) - PDF export

## License

This project is licensed under the MIT License.

> **Note:** The upstream Phomymo repository does not include a LICENSE file, though the README claims MIT. This project inherits that intent.

</details>

# Draw.io Diagram Skill

A skill that generates draw.io diagrams as `.drawio` files and optionally exports them to PNG/SVG/PDF.

## When to Use

Whenever you need a diagram:
- System architecture diagrams
- Flowcharts
- ER diagrams
- Sequence diagrams
- Network topology
- Any other diagram that draw.io can render

## Usage

### Basic Usage (`.drawio` file generation)

```
Create a login flowchart with drawio
```

→ Generates `login-flow.drawio` which opens in draw.io.

### Export as Image

```
Create an architecture diagram as drawio png
```

→ Generates `architecture-diagram.drawio.png`. PNG/SVG/PDF exports all embed the draw.io XML, so they remain editable.

### Format Examples

| Request | Output File |
|---------|-------------|
| `drawio flowchart` | `flowchart.drawio` |
| `drawio png login flow` | `login-flow.drawio.png` |
| `drawio svg ER diagram` | `er-diagram.drawio.svg` |
| `drawio pdf architecture overview` | `architecture-overview.drawio.pdf` |

## Prerequisites

- Export (PNG/SVG/PDF) requires [draw.io Desktop](https://github.com/jgraph/drawio-desktop/releases) to be installed
- Generating `.drawio` files only does not require installation

## Features

- Generates native draw.io XML format for full editability
- Exported PNG/SVG/PDF embed XML — open in draw.io to edit again
- Supports advanced draw.io features: containers, swimlanes, groups

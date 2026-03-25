# flutter-ui-components-skills

Skills for Syncfusion Flutter widgets, designed for use with AI coding assistants.

This repository contains 14 AI-ready skill guides for working with Syncfusion Flutter widgets. Each skill includes a `SKILL.md` file that AI coding assistants can read automatically, plus a `references/` subfolder with detailed documentation covering setup, usage patterns, customization, and feature-specific implementation guides.

## Quick Start

### Option 1: Using npx (Recommended)

```bash
npx skills add https://github.com/syncfusion/flutter-ui-components-skills
```

This will automatically add the skills to your workspace.

### Option 2: Manual Installation

**1. Clone this repository**
```bash
git clone https://github.com/syncfusion/flutter-ui-components-skills.git
```

**2. Add it to your VS Code workspace**

Open your `.code-workspace` file (or create one) and add this repo as a second root folder:
```json
{
  "folders": [
    { "path": "/path/to/your-flutter-app" },
    { "path": "/path/to/flutter-ui-components-skills" }
  ]
}
```

**3. Start asking questions**

Your AI assistant will automatically detect and apply the relevant skill based on your prompt:
```text
How do I add grouping to the Syncfusion DataGrid?
How do I configure SfCalendar for week view?
How do I apply a custom theme to SfCartesianChart?
```

No configuration required. Skills are loaded automatically from the workspace.

---

## Prerequisites

- An AI coding assistant that supports skills/context files (e.g., GitHub Copilot, Cursor, or similar tools)
- [Flutter SDK](https://flutter.dev/docs/get-started/install)
- A Syncfusion license key ([free community license available](https://www.syncfusion.com/products/communitylicense))

## How These Skills Work

Each `SKILL.md` file contains a `description` field in its YAML frontmatter. AI coding assistants read this description to decide when to automatically apply a skill during a conversation. When you ask about a specific Syncfusion widget — for example, "How do I add sorting to my DataGrid?" — the AI assistant detects the match and loads the corresponding skill to guide its response.

You can also reference a skill explicitly by mentioning the component or widget by name in your prompt.

### Example Prompts

```text
How do I bind data to the Syncfusion DataGrid in Flutter?
```
→ The AI assistant loads the DataGrid skill and uses its getting-started and data-binding reference docs.

```text
I need to add a date range picker to my app.
```
→ The AI assistant loads the Calendar & Date Range Picker skill.

```text
Help me create a sales funnel visualization for my dashboard.
```
→ The AI assistant loads the Funnel Chart skill.

### Using Reference Files

Each `references/` subfolder contains deeper implementation guides. When the AI assistant loads a skill, it can also pull in these files when you ask follow-up questions:

```text
Show me how to export the DataGrid to Excel.
```
→ The AI assistant uses `references/export-to-excel.md` from the DataGrid skill for the detailed answer.

## Skill File Structure

Every skill folder follows this layout:

```text
skills/
└── syncfusion-flutter-<component>/
    ├── SKILL.md                  ← Loaded by AI assistant; contains description, overview, and navigation links
    └── references/
        ├── getting-started.md    ← Installation, pubspec.yaml setup, basic usage
        ├── <feature-area>.md
        └── ...                   ← Additional reference files per component
```

`SKILL.md` is loaded automatically by the AI assistant; it contains the `description` trigger field, component overview, and links to all reference files in the skill. The `references/` folder contains focused Markdown files covering specific Flutter feature areas of the component.

## Repository Structure

```text
README.md
skills/
    syncfusion-flutter-barcode-generator/
    syncfusion-flutter-calendar-datepicker/
    syncfusion-flutter-cartesian-charts/
    syncfusion-flutter-chat/
    syncfusion-flutter-circular-charts/
    syncfusion-flutter-datagrid/
    syncfusion-flutter-funnel-charts/
    syncfusion-flutter-gauges/
    syncfusion-flutter-maps/
    syncfusion-flutter-pyramid-charts/
    syncfusion-flutter-signature-pad/
    syncfusion-flutter-sliders/
    syncfusion-flutter-spark-charts/
    syncfusion-flutter-treemap/
    ... (one folder per widget, 14 total)
```

## Skills Index

| Skill | Component(s) | Reference Files | Key Topics |
|---|---|:---:|---|
| [Barcode Generator](skills/syncfusion-flutter-barcode-generator/SKILL.md) | `SfBarcodeGenerator` | 4 | 1D/2D barcodes, QR Code, Data Matrix, Code128, EAN, customization |
| [Calendar & Date Range Picker](skills/syncfusion-flutter-calendar-datepicker/SKILL.md) | `SfCalendar`, `SfDateRangePicker` | 13 | Calendar views, appointments, date selection modes, recurrence, localization |
| [Cartesian Charts](skills/syncfusion-flutter-cartesian-charts/SKILL.md) | `SfCartesianChart` | 15 | 30+ chart types, axis types, zoom/pan, technical indicators, trendlines, export |
| [Chat & AI AssistView](skills/syncfusion-flutter-chat/SKILL.md) | `SfChat`, `SfAIAssistView` | 10 | Conversations, AI request/response, composer, toolbar actions, suggestion chips |
| [Circular Charts](skills/syncfusion-flutter-circular-charts/SKILL.md) | `SfCircularChart` | 8 | Pie, doughnut, radial bar, data labels, explode, legend, export |
| [Data Grid](skills/syncfusion-flutter-datagrid/SKILL.md) | `SfDataGrid` | 29 | Columns, sorting, filtering, grouping, editing, paging, freeze panes, export to Excel/PDF |
| [Funnel Chart](skills/syncfusion-flutter-funnel-charts/SKILL.md) | `SfFunnelChart` | 16 | Funnel customization, segment explode, data labels, legend, tooltip, export |
| [Gauges](skills/syncfusion-flutter-gauges/SKILL.md) | `SfRadialGauge`, `SfLinearGauge` | 11 | Radial/linear measurement, pointers, ranges, animation, interaction |
| [Maps](skills/syncfusion-flutter-maps/SKILL.md) | `SfMaps` | 15 | Shape layers, tile layers, markers, bubbles, color mapping, vector layers, zoom/pan |
| [Pyramid Chart](skills/syncfusion-flutter-pyramid-charts/SKILL.md) | `SfPyramidChart` | 7 | Linear/surface modes, segment customization, data labels, legend, export |
| [Signature Pad](skills/syncfusion-flutter-signature-pad/SKILL.md) | `SfSignaturePad` | 7 | Stroke capture, touch/stylus input, export to PNG/JPEG, validation, callbacks |
| [Sliders](skills/syncfusion-flutter-sliders/SKILL.md) | `SfSlider`, `SfRangeSlider`, `SfRangeSelector` | 13 | Single/range value selection, drag modes, chart integration, accessibility |
| [Spark Charts](skills/syncfusion-flutter-spark-charts/SKILL.md) | `SfSparkLineChart`, `SfSparkAreaChart`, `SfSparkBarChart`, `SfSparkWinLossChart` | 10 | Micro/inline charts, trend visualization, KPI indicators, markers, trackball |
| [TreeMap](skills/syncfusion-flutter-treemap/SKILL.md) | `SfTreemap` | 9 | Squarified/Slice/Dice layouts, hierarchical data, drilldown, color mapping, legend |

## Skill Index

### Data Visualization

- [Cartesian Charts](skills/syncfusion-flutter-cartesian-charts/SKILL.md)
- [Circular Charts](skills/syncfusion-flutter-circular-charts/SKILL.md)
- [Funnel Chart](skills/syncfusion-flutter-funnel-charts/SKILL.md)
- [Pyramid Chart](skills/syncfusion-flutter-pyramid-charts/SKILL.md)
- [Gauges (Radial & Linear)](skills/syncfusion-flutter-gauges/SKILL.md)
- [Maps](skills/syncfusion-flutter-maps/SKILL.md)
- [Spark Charts](skills/syncfusion-flutter-spark-charts/SKILL.md)
- [Barcode Generator](skills/syncfusion-flutter-barcode-generator/SKILL.md)
- [TreeMap](skills/syncfusion-flutter-treemap/SKILL.md)

### Grid

- [Data Grid](skills/syncfusion-flutter-datagrid/SKILL.md)

### Calendars

- [Calendar & Date Range Picker](skills/syncfusion-flutter-calendar-datepicker/SKILL.md)

### Sliders

- [Sliders (Slider, Range Slider, Range Selector)](skills/syncfusion-flutter-sliders/SKILL.md)

### Conversational UI

- [Chat & AI AssistView](skills/syncfusion-flutter-chat/SKILL.md)

### Viewer

- [Signature Pad](skills/syncfusion-flutter-signature-pad/SKILL.md)

## Coverage Summary

- **14 skills** covering the core Syncfusion Flutter widget library
- **167 reference files** across all skills
- Components range from focused (Barcode Generator — 4 refs) to comprehensive (Data Grid — 29 refs)

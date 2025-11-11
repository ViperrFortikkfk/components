# LARC Components

[![Version](https://img.shields.io/npm/v/@larcjs/components.svg)](https://www.npmjs.com/package/@larcjs/components)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Security](https://img.shields.io/badge/security-audited-brightgreen.svg)](docs/COMPONENT_SECURITY_AUDIT.md)

> **Production-ready UI web components** built on the PAN messaging bus

A comprehensive library of framework-agnostic UI components that communicate via the PAN (Page Area Network) messaging system. Build complex UIs with zero coupling between components.

## Features

- 🎨 **50+ Production Components** — Tables, forms, grids, charts, modals, and more
- 🔌 **Zero Coupling** — Components communicate only through PAN messages
- 🎯 **Framework Agnostic** — Works with vanilla JS, React, Vue, Lit, Angular
- 🔒 **Security Audited** — 0 critical vulnerabilities ([full audit](docs/COMPONENT_SECURITY_AUDIT.md))
- 🎨 **Themeable** — CSS custom properties for complete styling control
- ♿ **Accessible** — ARIA attributes and keyboard navigation
- 📦 **No Build Required** — Load components on demand via CDN

## Installation

```bash
npm install @larcjs/components @larcjs/core
```

## Quick Start

### CDN Usage (Recommended)

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <!-- Load LARC Core autoloader -->
  <script type="module" src="https://unpkg.com/@larcjs/core/src/pan.js"></script>
</head>
<body>
  <!-- Components load automatically on demand -->
  <pan-data-table
    resource="users"
    columns='["id", "name", "email"]'>
  </pan-data-table>

  <!-- Inspector for debugging -->
  <pan-inspector></pan-inspector>
</body>
</html>
```

### Module Usage

```javascript
import '@larcjs/core';
import '@larcjs/components/pan-data-table';
import '@larcjs/components/pan-form';

// Components are now registered and ready to use
```

## Core Components

### Data Display

#### `<pan-data-table>`
Enterprise-grade data grid with sorting, filtering, pagination, and inline editing.

```html
<pan-data-table
  resource="products"
  columns='["id", "name", "price", "stock"]'
  sortable="true"
  filterable="true"
  editable="true">
</pan-data-table>
```

**Features:**
- Virtual scrolling for large datasets
- Inline editing with validation
- Multi-column sorting
- Advanced filtering
- Export to CSV/Excel
- Responsive design

#### `<pan-grid>`
Flexible data grid with drag-and-drop reordering and customizable layouts.

```html
<pan-grid
  resource="cards"
  layout="masonry"
  columns="3">
</pan-grid>
```

#### `<pan-chart>`
Interactive charts powered by Chart.js integration.

```html
<pan-chart
  type="line"
  topic="analytics.data"
  x-axis="date"
  y-axis="sales">
</pan-chart>
```

### Forms & Input

#### `<pan-form>`
Schema-driven form generator with validation and error handling.

```html
<pan-form
  resource="user"
  schema-topic="user.schema"
  submit-topic="user.save">
</pan-form>
```

**Features:**
- JSON Schema validation
- Auto-generated fields from schema
- Real-time validation
- File upload support
- Custom field templates

#### `<pan-dropdown>`
Searchable dropdown with keyboard navigation.

```html
<pan-dropdown
  topic="country.select"
  options-topic="countries.list.state"
  placeholder="Select country">
</pan-dropdown>
```

#### `<pan-date-picker>`
Accessible date/time picker with localization.

```html
<pan-date-picker
  topic="booking.date"
  min="2024-01-01"
  format="YYYY-MM-DD">
</pan-date-picker>
```

### Data Providers

#### `<pan-data-provider>`
Connects to REST APIs and manages data state.

```html
<pan-data-provider
  resource="users"
  endpoint="/api/users"
  auto-load="true">
</pan-data-provider>
```

**Features:**
- Automatic CRUD operations
- Request caching
- Optimistic updates
- Error handling
- Retry logic

#### `<pan-data-connector>`
Generic connector for any data source.

```html
<pan-data-connector
  resource="products"
  adapter="graphql"
  endpoint="https://api.example.com/graphql">
</pan-data-connector>
```

### UI Controls

#### `<pan-modal>`
Accessible modal dialogs with backdrop.

```html
<pan-modal id="confirmDialog" title="Confirm Action">
  <p>Are you sure you want to proceed?</p>
  <button slot="footer">Confirm</button>
</pan-modal>
```

#### `<pan-tabs>`
Tab navigation with lazy loading.

```html
<pan-tabs>
  <pan-tab label="Overview" active>Content 1</pan-tab>
  <pan-tab label="Details">Content 2</pan-tab>
  <pan-tab label="Settings">Content 3</pan-tab>
</pan-tabs>
```

#### `<pan-toolbar>`
Action toolbar with responsive overflow menu.

```html
<pan-toolbar>
  <button data-action="save">Save</button>
  <button data-action="delete">Delete</button>
  <button data-action="export">Export</button>
</pan-toolbar>
```

### Developer Tools

#### `<pan-inspector>`
Real-time message inspector for debugging (DevTools-style UI).

```html
<pan-inspector
  position="bottom"
  height="300px"
  filter="user.*">
</pan-inspector>
```

**Features:**
- Message filtering by topic
- Payload inspection
- Message replay
- Performance metrics
- Export message logs

## Message Contracts

Components communicate via standardized topic patterns:

### List Operations
```javascript
// Request list
bus.publish('users.list.get', {});

// Receive list state
bus.subscribe('users.list.state', (msg) => {
  console.log('Users:', msg.payload.items);
});
```

### Item Operations
```javascript
// Select an item
bus.publish('users.item.select', { id: 123 });

// Request item details
await bus.request('users.item.get', { id: 123 });

// Save item
await bus.request('users.item.save', {
  item: { id: 123, name: 'Alice' }
});

// Delete item
await bus.request('users.item.delete', { id: 123 });
```

## Theming

Customize appearance using CSS custom properties:

```css
:root {
  /* Colors */
  --pan-primary-color: #007bff;
  --pan-secondary-color: #6c757d;
  --pan-success-color: #28a745;
  --pan-danger-color: #dc3545;

  /* Typography */
  --pan-font-family: system-ui, sans-serif;
  --pan-font-size: 14px;

  /* Spacing */
  --pan-spacing-sm: 8px;
  --pan-spacing-md: 16px;
  --pan-spacing-lg: 24px;

  /* Borders */
  --pan-border-radius: 4px;
  --pan-border-color: #dee2e6;
}
```

See [Theme System Documentation](docs/THEME_SYSTEM.md) for complete customization guide.

## Security

All components have been security audited:
- ✅ **0 critical vulnerabilities**
- ✅ **XSS protection** — Input sanitization on all user-editable fields
- ✅ **CSP compliant** — No inline scripts or styles
- ✅ **Safe defaults** — Secure configuration out of the box

See [Security Audit Report](docs/COMPONENT_SECURITY_AUDIT.md) for details.

## Browser Support

- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Opera 76+

## Component Catalog

View all components with live examples:
- [Gallery](https://larcjs.github.io/site/gallery.html)
- [Component Documentation](https://larcjs.github.io/site/docs/)
- [Interactive Demos](https://larcjs.github.io/examples/)

## Related Packages

- **[@larcjs/core](https://github.com/larcjs/core)** — Core PAN messaging bus
- **[@larcjs/devtools](https://github.com/larcjs/devtools)** — Chrome DevTools extension
- **[@larcjs/examples](https://github.com/larcjs/examples)** — Demo applications

## Contributing

Contributions are welcome! Please see our [Contributing Guide](CONTRIBUTING.md).

## License

MIT © Chris Robison

## Support

- 📖 [Documentation](https://larcjs.github.io/site/)
- 💬 [Discussions](https://github.com/larcjs/components/discussions)
- 🐛 [Issue Tracker](https://github.com/larcjs/components/issues)

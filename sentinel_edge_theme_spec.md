# Sentinel Edge — CSS Theme Switcher Spec
## For Claude Code implementation

---

## Overview

This spec defines a complete CSS theme system for the Sentinel Edge local web dashboard (aiohttp, Python, IoT edge device). The design language is sourced directly from Grafana 13 F12 CSS inspection. The implementation uses CSS custom properties on `[data-theme]` with a dropdown switcher, localStorage persistence, and two themes.

**Source of truth:** Grafana 13 F12 CSS dump, cross-referenced across multiple page states.

---

## Step 1 — Refactor existing CSS to custom properties

Replace ALL hardcoded values in the existing stylesheet with `var(--x)` references. Do not leave any hardcoded colours, font sizes, spacing values, or border widths.

---

## Step 2 — Add fonts to `<head>`

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Roboto+Mono:wght@400;600&display=swap" rel="stylesheet">
```

---

## Step 3 — themes.css

Create a `themes.css` file. Apply the default theme to `:root` and override per `[data-theme="x"]`.

### 3.1 — dark (default)

Sourced directly from Grafana 13 F12 session. This is the canonical reference.

```css
:root,
[data-theme="dark"] {

  /* ── BACKGROUNDS ── */
  --bg:                    #111217;   /* body / page root */
  --bg-panel:              #181b1f;   /* sidebar, navbar, panel containers */
  --bg-card:               #22252b;   /* cards, tooltips, dropdowns */
  --bg-secondary:          #2c2f35;   /* secondary button bg, input addon bg */
  --bg-hover:              #383b42;   /* hover state on interactive surfaces */
  --bg-canvas:             #181b1f;   /* dashboard canvas */

  /* ── TEXT ── */
  --text:                  #ccccdc;
  --text-secondary:        rgba(204, 204, 220, 0.65);
  --text-disabled:         rgba(204, 204, 220, 0.61);
  --text-link:             #6e9fff;
  --text-placeholder:      rgba(204, 204, 220, 0.61);
  --text-maxcontrast:      #ffffff;

  /* ── BORDERS ── */
  --border:                rgba(204, 204, 220, 0.12);   /* subtle — panels, dividers */
  --border-medium:         rgba(204, 204, 220, 0.20);   /* inputs default */
  --border-strong:         rgba(204, 204, 220, 0.30);   /* inputs hover */
  --border-solid:          #34343b;                     /* solid opaque dividers */

  /* ── ACCENT / PRIMARY ── */
  --accent:                #3d71d9;
  --accent-hover:          #5a86de;
  --accent-light:          #6e9fff;
  --accent-dim:            rgba(61, 113, 217, 0.15);

  /* ── SEMANTIC ── */
  --success:               #73bf69;
  --warning:               #f2cc0c;
  --error:                 #f2495c;
  --danger:                #ff5286;   /* alert text / icon colour */
  --info:                  #6e9fff;
  --orange:                #ff9830;

  /* ── ALERT STATES (confirmed from .alert-state-* classes) ── */
  --alert-ok:              #6ccf8e;
  --alert-warning:         #fbad37;
  --alert-critical:        #ff5286;
  --alert-paused:          rgba(204, 204, 220, 0.65);
  --alert-pending:         rgba(204, 204, 220, 0.65);

  /* ── LOG LEVEL COLOURS ── */
  --log-critical:          #b877d9;
  --log-error:             #ff5286;
  --log-warning:           #fbad37;
  --log-info:              #6ccf8e;
  --log-debug:             #6e9fff;

  /* ── BUTTONS ── */
  --btn-primary-bg:        #3d71d9;
  --btn-primary-bg-hover:  #5a86de;
  --btn-primary-text:      #ffffff;
  --btn-secondary-bg:      #2c2f35;
  --btn-secondary-bg-hover:#383b42;
  --btn-secondary-text:    #ccccdc;
  --btn-secondary-border:  rgba(204, 204, 220, 0.08);
  --btn-danger-bg:         #d10e5c;
  --btn-danger-bg-hover:   #d73274;
  --btn-danger-text:       #ffffff;
  --btn-disabled-bg:       rgba(204, 204, 220, 0.04);
  --btn-disabled-text:     rgba(204, 204, 220, 0.61);
  --btn-ghost-text:        rgba(204, 204, 220, 0.65);
  --btn-ghost-bg-hover:    rgba(204, 204, 220, 0.16);
  --btn-outline-border:    rgba(204, 204, 220, 0.30);
  --btn-outline-bg-hover:  rgba(204, 204, 220, 0.08);

  /* ── INPUTS ── */
  --input-bg:              #111217;
  --input-border:          rgba(204, 204, 220, 0.20);
  --input-border-hover:    rgba(204, 204, 220, 0.30);
  --input-text:            #ccccdc;
  --input-placeholder:     rgba(204, 204, 220, 0.61);
  --input-disabled-bg:     rgba(204, 204, 220, 0.04);
  --input-disabled-border: rgba(204, 204, 220, 0.04);
  --input-height:          32px;
  --input-radius:          6px;

  /* ── NAVIGATION / SIDEBAR ── */
  --nav-bg:                #181b1f;
  --nav-border:            rgba(204, 204, 220, 0.12);
  --nav-item-color:        rgba(204, 204, 220, 0.65);
  --nav-item-color-active: rgb(204, 204, 220);
  --nav-item-hover-bg:     rgba(204, 204, 220, 0.16);
  --nav-active-bg:         rgba(204, 204, 220, 0.12);
  --nav-active-border-radius: 6px;
  --nav-active-gradient:   linear-gradient(0.01deg, rgb(245, 95, 62) 0.01%, rgb(255, 136, 51) 99.99%);

  /* ── PANELS / CARDS ── */
  --panel-bg:              #181b1f;
  --panel-border:          rgba(204, 204, 220, 0.12);
  --panel-shadow:          rgba(1, 4, 9, 0.75) 0px 4px 8px;
  --panel-header-bg:       #181b1f;

  /* ── TABLES ── */
  --table-row-odd:         #1c1f23;
  --table-row-hover:       #23262a;
  --table-header-bg:       #181b1f;
  --table-border:          rgba(204, 204, 220, 0.20);

  /* ── TOOLTIPS ── */
  --tooltip-bg:            #22252b;
  --tooltip-text:          #ccccdc;
  --tooltip-border:        #22252b;
  --tooltip-shadow:        rgba(1, 4, 9, 0.75) 0px 4px 8px;

  /* ── LOG VIEWER ── */
  --log-row-bg:            #181b1f;
  --log-row-bg-hover:      #000000;   /* drops to pure black on hover — confirmed Grafana behaviour */
  --log-row-selected-bg:   rgba(61, 113, 217, 0.25);
  --log-row-border:        rgba(204, 204, 220, 0.30);
  --log-timestamp-color:   rgba(204, 204, 220, 0.65);
  --log-body-color:        #ccccdc;
  --log-key-color:         rgba(204, 204, 220, 0.65);
  --log-key-weight:        500;
  --log-value-color:       #ccccdc;
  --log-string-color:      #ccccdc;
  --log-number-color:      #6e9fff;
  --log-duration-color:    #6ccf8e;   /* durations, sizes, UUIDs */
  --log-uuid-color:        #6ccf8e;
  --log-method-color:      #5a86de;   /* HTTP methods */
  --log-search-match-bg:   #ff9900;
  --log-search-match-text: #000000;
  --log-streaming-color:   #6ccf8e;
  --log-font:              'Roboto Mono', monospace;
  --log-font-size-sm:      0.857143rem;   /* 12px */
  --log-font-size-md:      14px;
  --log-line-height:       18px;

  /* ── FOCUS RING ── */
  --focus-ring:            rgb(17, 18, 23) 0px 0px 0px 2px, rgb(61, 113, 217) 0px 0px 0px 4px;

  /* ── SCROLLBAR ── */
  --scrollbar-thumb:       rgba(204, 204, 220, 0.30);
  --scrollbar-track:       transparent;

  /* ── TYPOGRAPHY ── */
  --font:                  'Inter', Helvetica, Arial, sans-serif;
  --font-mono:             'Roboto Mono', monospace;
  --font-size:             14px;
  --font-size-sm:          12px;
  --font-size-xs:          11px;
  --font-size-lg:          16px;
  --font-weight-normal:    400;
  --font-weight-medium:    500;
  --font-weight-semi:      600;
  --line-height:           1.57143;
  --line-height-sm:        1.5;
  --letter-spacing:        0.01071em;
  --letter-spacing-sm:     0.0125em;

  /* ── HEADING SCALE (rem, base 14px) ── */
  --h1-size:               2rem;         /* 28px */
  --h1-line-height:        1.14286;
  --h1-weight:             400;
  --h1-letter-spacing:     -0.00893em;

  --h2-size:               1.71429rem;   /* 24px */
  --h2-line-height:        1.16667;
  --h2-weight:             400;
  --h2-letter-spacing:     0em;

  --h3-size:               1.57143rem;   /* 22px */
  --h3-line-height:        1.09091;
  --h3-weight:             400;
  --h3-letter-spacing:     0em;

  --h4-size:               1.28571rem;   /* 18px */
  --h4-line-height:        1.22222;
  --h4-weight:             400;
  --h4-letter-spacing:     0.01389em;

  --h5-size:               1.14286rem;   /* 16px */
  --h5-line-height:        1.375;
  --h5-weight:             400;
  --h5-letter-spacing:     0em;

  --h6-size:               1rem;         /* 14px */
  --h6-line-height:        1.57143;
  --h6-weight:             500;
  --h6-letter-spacing:     0.01071em;

  /* ── SPACING SCALE ── */
  --space-xs:              4px;
  --space-sm:              8px;
  --space-md:              16px;
  --space-lg:              24px;
  --space-xl:              32px;
  --space-xxl:             48px;

  /* ── LAYOUT ── */
  --sidebar-width:         300px;
  --navbar-height:         48px;
  --panel-header-height:   32px;
  --breadcrumb-height:     48px;

  /* ── BORDER RADIUS SCALE ── */
  --radius-sm:             2px;
  --radius:                6px;
  --radius-md:             6px;
  --radius-lg:             10px;
  --radius-pill:           9999px;

  /* ── LINE / BORDER WIDTHS ── */
  --border-width:          1px;
  --border-width-active:   2px;   /* nav indicator stripe, tab underline */
  --divider-width:         1px;

  /* ── COMPONENT HEIGHTS ── */
  --btn-height-sm:         24px;
  --btn-height-md:         32px;
  --btn-height-lg:         52px;
  --toolbar-height:        32px;
  --icon-btn-size:         24px;
  --icon-btn-size-lg:      26px;

  /* ── COMPONENT PADDING ── */
  --btn-padding-sm:        0px 7px;
  --btn-padding-md:        0px 15px;
  --btn-padding-icon:      0px 8px;

  /* ── TRANSITIONS ── */
  --btn-transition:        background-color 250ms cubic-bezier(0.4, 0, 0.2, 1),
                           border-color 250ms cubic-bezier(0.4, 0, 0.2, 1),
                           color 250ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-fast:       0.2s;
  --transition-normal:     0.3s;

  /* ── Z-INDEX SCALE ── */
  --z-sidebar:             2;
  --z-navbar:              1000;
  --z-overlay:             1001;
  --z-dropdown:            1030;
  --z-tooltip:             1040;
  --z-modal:               1050;
  --z-notification:        1061;

  /* ── GRADIENTS ── */
  --gradient-active-tab:      linear-gradient(270deg, rgb(245, 95, 62) 0%, rgb(255, 136, 51) 100%);
  --gradient-nav-indicator:   linear-gradient(0.01deg, rgb(245, 95, 62) 0.01%, rgb(255, 136, 51) 99.99%);

  /* ── COLOR SCHEME ── */
  --color-scheme: dark;   /* native browser UI inherits dark mode */
}
```

---

### 3.2 — light

```css
[data-theme="light"] {

  /* ── BACKGROUNDS ── */
  --bg:                    #f4f5f5;
  --bg-panel:              #ffffff;
  --bg-card:               #f0f1f2;
  --bg-secondary:          #e5e7e9;
  --bg-hover:              #dde0e3;
  --bg-canvas:             #f4f5f5;

  /* ── TEXT ── */
  --text:                  #1f2933;
  --text-secondary:        rgba(31, 41, 51, 0.65);
  --text-disabled:         rgba(31, 41, 51, 0.61);
  --text-link:             #3d71d9;
  --text-placeholder:      rgba(31, 41, 51, 0.61);
  --text-maxcontrast:      #000000;

  /* ── BORDERS ── */
  --border:                rgba(31, 41, 51, 0.12);
  --border-medium:         rgba(31, 41, 51, 0.20);
  --border-strong:         rgba(31, 41, 51, 0.30);
  --border-solid:          #d0d1d3;

  /* ── ACCENT / PRIMARY ── */
  --accent:                #3d71d9;
  --accent-hover:          #5a86de;
  --accent-light:          #6e9fff;
  --accent-dim:            rgba(61, 113, 217, 0.15);

  /* ── SEMANTIC ── */
  --success:               #52a323;
  --warning:               #cc9804;
  --error:                 #e02f44;
  --danger:                #e02f44;
  --info:                  #3d71d9;
  --orange:                #ff9830;

  /* ── ALERT STATES ── */
  --alert-ok:              #52a323;
  --alert-warning:         #cc9804;
  --alert-critical:        #e02f44;
  --alert-paused:          rgba(31, 41, 51, 0.65);
  --alert-pending:         rgba(31, 41, 51, 0.65);

  /* ── LOG LEVEL COLOURS ── */
  --log-critical:          #7c3aed;
  --log-error:             #e02f44;
  --log-warning:           #cc9804;
  --log-info:              #52a323;
  --log-debug:             #3d71d9;

  /* ── BUTTONS ── */
  --btn-primary-bg:        #3d71d9;
  --btn-primary-bg-hover:  #5a86de;
  --btn-primary-text:      #ffffff;
  --btn-secondary-bg:      #e5e7e9;
  --btn-secondary-bg-hover:#dde0e3;
  --btn-secondary-text:    #1f2933;
  --btn-secondary-border:  rgba(31, 41, 51, 0.08);
  --btn-danger-bg:         #e02f44;
  --btn-danger-bg-hover:   #d73274;
  --btn-danger-text:       #ffffff;
  --btn-disabled-bg:       rgba(31, 41, 51, 0.04);
  --btn-disabled-text:     rgba(31, 41, 51, 0.61);
  --btn-ghost-text:        rgba(31, 41, 51, 0.65);
  --btn-ghost-bg-hover:    rgba(31, 41, 51, 0.16);
  --btn-outline-border:    rgba(31, 41, 51, 0.30);
  --btn-outline-bg-hover:  rgba(31, 41, 51, 0.08);

  /* ── INPUTS ── */
  --input-bg:              #ffffff;
  --input-border:          rgba(31, 41, 51, 0.20);
  --input-border-hover:    rgba(31, 41, 51, 0.30);
  --input-text:            #1f2933;
  --input-placeholder:     rgba(31, 41, 51, 0.61);
  --input-disabled-bg:     rgba(31, 41, 51, 0.04);
  --input-disabled-border: rgba(31, 41, 51, 0.04);
  --input-height:          32px;
  --input-radius:          6px;

  /* ── NAVIGATION / SIDEBAR ── */
  --nav-bg:                #ffffff;
  --nav-border:            rgba(31, 41, 51, 0.12);
  --nav-item-color:        rgba(31, 41, 51, 0.65);
  --nav-item-color-active: rgb(31, 41, 51);
  --nav-item-hover-bg:     rgba(31, 41, 51, 0.16);
  --nav-active-bg:         rgba(31, 41, 51, 0.12);
  --nav-active-border-radius: 6px;
  --nav-active-gradient:   linear-gradient(0.01deg, rgb(245, 95, 62) 0.01%, rgb(255, 136, 51) 99.99%);

  /* ── PANELS / CARDS ── */
  --panel-bg:              #ffffff;
  --panel-border:          rgba(31, 41, 51, 0.12);
  --panel-shadow:          0 1px 3px rgba(31, 41, 51, 0.12);
  --panel-header-bg:       #ffffff;

  /* ── TABLES ── */
  --table-row-odd:         #f4f5f5;
  --table-row-hover:       #eaecef;
  --table-header-bg:       #f0f1f2;
  --table-border:          rgba(31, 41, 51, 0.20);

  /* ── TOOLTIPS ── */
  --tooltip-bg:            #1f2933;
  --tooltip-text:          #ffffff;
  --tooltip-border:        #1f2933;
  --tooltip-shadow:        rgba(0, 0, 0, 0.20) 0px 4px 8px;

  /* ── LOG VIEWER ── */
  --log-row-bg:            #ffffff;
  --log-row-bg-hover:      #f0f1f2;
  --log-row-selected-bg:   rgba(61, 113, 217, 0.12);
  --log-row-border:        rgba(31, 41, 51, 0.20);
  --log-timestamp-color:   rgba(31, 41, 51, 0.65);
  --log-body-color:        #1f2933;
  --log-key-color:         rgba(31, 41, 51, 0.65);
  --log-key-weight:        500;
  --log-value-color:       #1f2933;
  --log-string-color:      #1f2933;
  --log-number-color:      #3d71d9;
  --log-duration-color:    #52a323;
  --log-uuid-color:        #52a323;
  --log-method-color:      #5a86de;
  --log-search-match-bg:   #ff9900;
  --log-search-match-text: #000000;
  --log-streaming-color:   #52a323;
  --log-font:              'Roboto Mono', monospace;
  --log-font-size-sm:      0.857143rem;
  --log-font-size-md:      14px;
  --log-line-height:       18px;

  /* ── FOCUS RING ── */
  --focus-ring:            #f4f5f5 0px 0px 0px 2px, rgb(61, 113, 217) 0px 0px 0px 4px;

  /* ── SCROLLBAR ── */
  --scrollbar-thumb:       rgba(31, 41, 51, 0.30);
  --scrollbar-track:       transparent;

  /* ── All structural tokens identical to dark ── */
  --font:                  'Inter', Helvetica, Arial, sans-serif;
  --font-mono:             'Roboto Mono', monospace;
  --font-size:             14px;
  --font-size-sm:          12px;
  --font-size-xs:          11px;
  --font-size-lg:          16px;
  --font-weight-normal:    400;
  --font-weight-medium:    500;
  --font-weight-semi:      600;
  --line-height:           1.57143;
  --line-height-sm:        1.5;
  --letter-spacing:        0.01071em;
  --letter-spacing-sm:     0.0125em;
  --h1-size: 2rem;         --h1-line-height: 1.14286; --h1-weight: 400; --h1-letter-spacing: -0.00893em;
  --h2-size: 1.71429rem;   --h2-line-height: 1.16667; --h2-weight: 400; --h2-letter-spacing: 0em;
  --h3-size: 1.57143rem;   --h3-line-height: 1.09091; --h3-weight: 400; --h3-letter-spacing: 0em;
  --h4-size: 1.28571rem;   --h4-line-height: 1.22222; --h4-weight: 400; --h4-letter-spacing: 0.01389em;
  --h5-size: 1.14286rem;   --h5-line-height: 1.375;   --h5-weight: 400; --h5-letter-spacing: 0em;
  --h6-size: 1rem;         --h6-line-height: 1.57143; --h6-weight: 500; --h6-letter-spacing: 0.01071em;
  --space-xs: 4px; --space-sm: 8px; --space-md: 16px; --space-lg: 24px; --space-xl: 32px; --space-xxl: 48px;
  --sidebar-width: 300px; --navbar-height: 48px; --panel-header-height: 32px; --breadcrumb-height: 48px;
  --radius-sm: 2px; --radius: 6px; --radius-md: 6px; --radius-lg: 10px; --radius-pill: 9999px;
  --border-width: 1px; --border-width-active: 2px; --divider-width: 1px;
  --btn-height-sm: 24px; --btn-height-md: 32px; --btn-height-lg: 52px;
  --toolbar-height: 32px; --icon-btn-size: 24px; --icon-btn-size-lg: 26px;
  --btn-padding-sm: 0px 7px; --btn-padding-md: 0px 15px; --btn-padding-icon: 0px 8px;
  --btn-transition: background-color 250ms cubic-bezier(0.4, 0, 0.2, 1),
                    border-color 250ms cubic-bezier(0.4, 0, 0.2, 1),
                    color 250ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-fast: 0.2s; --transition-normal: 0.3s;
  --z-sidebar: 2; --z-navbar: 1000; --z-overlay: 1001; --z-dropdown: 1030;
  --z-tooltip: 1040; --z-modal: 1050; --z-notification: 1061;
  --gradient-active-tab:    linear-gradient(270deg, rgb(245, 95, 62) 0%, rgb(255, 136, 51) 100%);
  --gradient-nav-indicator: linear-gradient(0.01deg, rgb(245, 95, 62) 0.01%, rgb(255, 136, 51) 99.99%);

  /* ── COLOR SCHEME ── */
  --color-scheme: light;
}
```

---

## Step 4 — Component CSS rules

All rules use `var(--x)` exclusively. No hardcoded values.

### 4.1 — Base / reset

```css
html {
  font-size: var(--font-size);
  font-family: var(--font);
  line-height: var(--line-height);
  color: var(--text);
  background-color: var(--bg);
  box-sizing: border-box;
  scrollbar-width: thin;
  scrollbar-color: var(--scrollbar-thumb) var(--scrollbar-track);
  color-scheme: var(--color-scheme);   /* tells browser native UI (scrollbars, inputs) which scheme to use */
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);   /* disables blue tap flash on mobile/touch */
  direction: ltr;
}
*, *::before, *::after { box-sizing: inherit; }

body {
  font-family: var(--font);
  font-weight: var(--font-weight-normal);
  font-size: 1rem;
  line-height: var(--line-height);
  letter-spacing: var(--letter-spacing);
  color: var(--text);
  background-color: var(--bg);
  margin: 0;
  font-kerning: normal;
  font-variant-ligatures: no-contextual;   /* Grafana explicitly disables contextual ligatures */
}
```

### 4.2 — Typography

```css
h1, .h1 { font-size: var(--h1-size); line-height: var(--h1-line-height); font-weight: var(--h1-weight); letter-spacing: var(--h1-letter-spacing); font-family: var(--font); margin: 0 0 0.45em; color: var(--text); }
h2, .h2 { font-size: var(--h2-size); line-height: var(--h2-line-height); font-weight: var(--h2-weight); letter-spacing: var(--h2-letter-spacing); font-family: var(--font); margin: 0 0 0.45em; color: var(--text); }
h3, .h3 { font-size: var(--h3-size); line-height: var(--h3-line-height); font-weight: var(--h3-weight); letter-spacing: var(--h3-letter-spacing); font-family: var(--font); margin: 0 0 0.45em; color: var(--text); }
h4, .h4 { font-size: var(--h4-size); line-height: var(--h4-line-height); font-weight: var(--h4-weight); letter-spacing: var(--h4-letter-spacing); font-family: var(--font); margin: 0 0 0.45em; color: var(--text); }
h5, .h5 { font-size: var(--h5-size); line-height: var(--h5-line-height); font-weight: var(--h5-weight); letter-spacing: var(--h5-letter-spacing); font-family: var(--font); margin: 0 0 0.45em; color: var(--text); }
h6, .h6 { font-size: var(--h6-size); line-height: var(--h6-line-height); font-weight: var(--h6-weight); letter-spacing: var(--h6-letter-spacing); font-family: var(--font); margin: 0 0 0.45em; color: var(--text); }

p { margin: 0 0 var(--space-md); color: var(--text); }

small, .small {
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-normal);
  line-height: var(--line-height-sm);
  letter-spacing: var(--letter-spacing-sm);
}

a { color: var(--text-link); text-decoration: none; cursor: pointer; }
a:hover { text-decoration: underline; }

code, pre, kbd, samp {
  font-family: var(--font-mono);
  font-size: var(--font-size-sm);
  background-color: var(--bg-panel);
  color: var(--text);
  border: var(--border-width) solid var(--border-medium);
  border-radius: var(--radius);
}
code { white-space: nowrap; padding: 2px 5px; margin: 0 2px; }
pre  { display: block; margin: 0 0 var(--space-md); padding: 10px; overflow: auto; white-space: pre-wrap; }

hr { border: 0; border-top: var(--border-width) solid var(--border); margin: var(--space-md) 0; }
```

### 4.3 — Buttons

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-sm);
  font-family: var(--font);
  font-weight: var(--font-weight-medium);
  font-size: var(--font-size);
  height: var(--btn-height-md);
  padding: var(--btn-padding-md);
  line-height: calc(var(--btn-height-md) - 2px);
  border-radius: var(--radius);
  border: var(--border-width) solid transparent;
  cursor: pointer;
  vertical-align: middle;
  text-decoration: none;
  transition: var(--btn-transition);
  letter-spacing: var(--letter-spacing);
}
.btn:focus-visible { outline: transparent dotted 2px; outline-offset: 2px; box-shadow: var(--focus-ring); }
.btn:focus:not(:focus-visible) { outline: none; box-shadow: none; }
.btn:disabled, .btn[disabled] {
  cursor: not-allowed; box-shadow: none;
  background: var(--btn-disabled-bg); color: var(--btn-disabled-text);
  border-color: transparent; transition: none;
}

.btn-primary  { background: var(--btn-primary-bg);   color: var(--btn-primary-text); }
.btn-primary:hover  { background: var(--btn-primary-bg-hover);  box-shadow: rgba(1,4,9,0.75) 0px 1px 2px; }
.btn-primary:focus  { background: var(--btn-primary-bg-hover); }
.btn-primary:active { background: var(--btn-primary-bg); }

.btn-secondary { background: var(--btn-secondary-bg); color: var(--btn-secondary-text); border-color: var(--btn-secondary-border); }
.btn-secondary:hover,
.btn-secondary:focus  { background: var(--btn-secondary-bg-hover); border-color: var(--border-medium); }
.btn-secondary:active { background: var(--btn-secondary-bg); }

.btn-danger  { background: var(--btn-danger-bg); color: var(--btn-danger-text); }
.btn-danger:hover  { background: var(--btn-danger-bg-hover); box-shadow: rgba(1,4,9,0.75) 0px 1px 2px; }
.btn-danger:focus  { background: var(--btn-danger-bg-hover); }
.btn-danger:active { background: var(--btn-danger-bg); }

.btn-ghost  { background: transparent; color: var(--btn-ghost-text); border-color: transparent; }
.btn-ghost:hover  { background: var(--btn-ghost-bg-hover); color: var(--text); }
.btn-ghost:active { background: var(--btn-secondary-bg); }

.btn-outline { background: transparent; color: var(--text); border-color: var(--btn-outline-border); }
.btn-outline:hover,
.btn-outline:focus { background: var(--btn-outline-bg-hover); border-color: var(--border-strong); }

.btn-sm {
  height: var(--btn-height-sm);
  padding: var(--btn-padding-sm);
  font-size: var(--font-size-sm);
  line-height: calc(var(--btn-height-sm) - 2px);
}

.btn-icon { padding: var(--btn-padding-icon); width: var(--btn-height-md); justify-content: center; }
```

### 4.4 — Inputs

```css
.input,
input[type="text"],
input[type="number"],
input[type="email"],
input[type="password"],
input[type="search"],
select,
textarea {
  display: block;
  width: 100%;
  height: var(--input-height);
  padding: 0 var(--space-sm);
  font-family: var(--font);
  font-size: var(--font-size);
  font-weight: var(--font-weight-normal);
  line-height: var(--line-height);
  color: var(--input-text);
  background: var(--input-bg);
  border: var(--border-width) solid var(--input-border);
  border-radius: var(--input-radius);
  transition: border-color var(--transition-fast);
}
.input:hover, input:hover    { border-color: var(--input-border-hover); }
.input:focus, input:focus    { outline: transparent dotted 2px; outline-offset: 2px; box-shadow: var(--focus-ring); border-color: var(--accent-light); }
.input::placeholder          { color: var(--input-placeholder); opacity: 1; }
.input:disabled, input:disabled { background: var(--input-disabled-bg); color: var(--text-disabled); border-color: var(--input-disabled-border); cursor: not-allowed; }

textarea { height: auto; resize: vertical; padding: var(--space-sm); }

select {
  appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath fill='%236e6e8a' d='M1 1l5 5 5-5'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 8px center;
  padding-right: 28px;
}
```

### 4.5 — Tables

```css
table { width: 100%; border-collapse: collapse; font-family: var(--font); font-size: var(--font-size); }
thead { background: var(--table-header-bg); }
th {
  font-weight: var(--font-weight-medium);
  text-align: left;
  padding: var(--space-xs) var(--space-sm);
  color: var(--text);
  border-bottom: var(--border-width) solid var(--table-border);
  white-space: nowrap;
  line-height: 30px;
  height: 30px;
}
td {
  padding: var(--space-xs) var(--space-sm);
  color: var(--text);
  border-bottom: var(--border-width) solid var(--border);
  line-height: 30px;
  height: 30px;
  white-space: nowrap;
}
tbody tr:nth-of-type(odd) { background: var(--table-row-odd); }
tbody tr:hover            { background: var(--table-row-hover); }
```

### 4.6 — Panels / Cards

```css
.panel,
.card {
  background: var(--panel-bg);
  border: var(--border-width) solid var(--panel-border);
  border-radius: var(--radius);
  box-shadow: var(--panel-shadow);
  overflow: hidden;
}

.panel-header {
  display: flex;
  align-items: center;
  padding: 0 var(--space-sm);
  gap: var(--space-sm);
  height: var(--panel-header-height);
  background: var(--panel-header-bg);
  border-bottom: var(--border-width) solid var(--panel-border);
  font-weight: var(--font-weight-medium);
  font-size: 1rem;
  line-height: var(--line-height);
  letter-spacing: var(--letter-spacing);
}

.panel-title {
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: var(--text);
  font-weight: var(--font-weight-medium);
  font-size: 1rem;
}

.panel-content { padding: var(--space-md); }
```

### 4.7 — Navigation / Sidebar

```css
.sidebar {
  background: var(--nav-bg);
  border-right: var(--border-width) solid var(--nav-border);
  width: var(--sidebar-width);
  height: 100%;
  display: flex;
  flex-direction: column;
}

.nav-list {
  list-style: none;
  padding: var(--space-sm) var(--space-sm) var(--space-md) var(--space-xs);
  margin: 0;
  display: flex;
  flex-direction: column;
}

.nav-item {
  height: 32px;
  display: flex;
  align-items: center;
  position: relative;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: var(--space-sm);
  width: 100%;
  height: 100%;
  padding: 0 var(--space-sm);
  color: var(--nav-item-color);
  text-decoration: none;
  font-size: var(--font-size);
  border-radius: var(--nav-active-border-radius);
  transition: background var(--transition-fast), color var(--transition-fast);
}
.nav-link:hover {
  background: var(--nav-item-hover-bg);
  color: var(--nav-item-color-active);
}
.nav-link.active {
  background: var(--nav-active-bg);
  color: var(--nav-item-color-active);
  border-top-right-radius: var(--nav-active-border-radius);
  border-bottom-right-radius: var(--nav-active-border-radius);
}

/* Orange/amber left-edge gradient stripe on active nav item */
.nav-link.active::before {
  content: ' ';
  display: block;
  position: absolute;
  left: 0;
  top: 0;
  width: var(--border-width-active);
  height: 100%;
  background-image: var(--nav-active-gradient);
  border-radius: var(--radius);
}

.nav-section-header {
  height: var(--breadcrumb-height);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--space-sm);
  border-bottom: var(--border-width) solid var(--nav-border);
  flex-shrink: 0;
}
```

### 4.8 — Navbar / Topbar

```css
.navbar {
  display: flex;
  position: fixed;
  z-index: var(--z-navbar);
  left: 0; right: 0; top: 0;
  background: var(--bg-panel);
  height: var(--navbar-height);
  border-bottom: var(--border-width) solid var(--border);
}
.navbar-inner {
  display: flex;
  align-items: center;
  height: 100%;
  padding: 0 var(--space-sm);
  gap: var(--space-sm);
}
```

### 4.9 — Badges

```css
.badge {
  display: inline-flex;
  align-items: center;
  padding: 1px var(--space-xs);
  border-radius: 4px;
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-normal);
  line-height: var(--line-height-sm);
  gap: var(--space-xs);
}
.badge-success { background: rgba(115,191,105,0.15); border: var(--border-width) solid rgba(115,191,105,0.25); color: var(--success); }
.badge-warning { background: rgba(242,204,12,0.15);  border: var(--border-width) solid rgba(242,204,12,0.25);  color: var(--warning); }
.badge-error   { background: rgba(242,73,92,0.15);   border: var(--border-width) solid rgba(242,73,92,0.25);   color: var(--error); }
.badge-info    { background: rgba(87,148,242,0.15);  border: var(--border-width) solid rgba(87,148,242,0.25);  color: var(--info); }

.alert-state-ok       { color: var(--alert-ok);       font-weight: var(--font-weight-medium); }
.alert-state-warning  { color: var(--alert-warning);  font-weight: var(--font-weight-medium); }
.alert-state-critical { color: var(--alert-critical); font-weight: var(--font-weight-medium); }
.alert-state-paused   { color: var(--alert-paused);   font-weight: var(--font-weight-medium); }
.alert-state-pending  { color: var(--alert-pending);  font-weight: var(--font-weight-medium); }
```

### 4.10 — Tooltip

```css
.tooltip {
  background: var(--tooltip-bg);
  color: var(--tooltip-text);
  border: var(--border-width) solid var(--tooltip-border);
  border-radius: var(--radius);
  box-shadow: var(--tooltip-shadow);
  font-size: var(--font-size-sm);
  padding: var(--space-xs) var(--space-sm);
  max-width: 400px;
  overflow-wrap: break-word;
  z-index: var(--z-tooltip);
  pointer-events: none;
}
```

### 4.11 — Dividers

```css
.divider {
  height: var(--divider-width);
  background: var(--border);
  margin: var(--space-sm) 0;
  border: none;
}
.divider-vertical {
  width: var(--divider-width);
  height: 24px;
  background: var(--border-medium);
  flex-shrink: 0;
}
```

### 4.12 — Active tab / underline indicator

```css
.tab-active::before {
  display: block;
  content: ' ';
  position: absolute;
  left: 0; right: 0;
  height: var(--border-width-active);
  bottom: 0;
  border-radius: var(--radius);
  background-image: var(--gradient-active-tab);
}
```

### 4.13 — Log Viewer

Log rows use monospace font at 12px, compact 18px line height. Hover drops to near-black in dark theme / light grey in light theme. Timestamps are fixed-width, secondary text colour. Syntax tokens mirror Grafana's log viewer colouring. Search match highlight is orange with black text.

**Note on Grafana's virtualised rendering:** In Grafana, log rows are `position: absolute` with `width: max-content; min-width: 100%` — they are rendered into a virtualised scroll container and only visible rows exist in the DOM. For Sentinel Edge, use normal document flow (`display: flex`) unless the log volume is large enough to require virtualisation. The token values and visual rules below apply regardless of rendering strategy.

```css
/* ── Container ── */
.log-viewer {
  font-family: var(--log-font);
  font-size: var(--log-font-size-sm);
  line-height: var(--log-line-height);
  background: var(--log-row-bg);
  overflow-x: hidden;
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: var(--scrollbar-thumb) var(--scrollbar-track);
}

/* ── Individual row ── */
.log-row {
  display: flex;
  flex-direction: row;
  gap: var(--space-xs);
  color: var(--log-body-color);
  font-family: var(--log-font);
  word-break: break-all;
  position: relative;
  padding-bottom: 6px;
  min-height: var(--log-line-height);
}
.log-row:hover    { background: var(--log-row-bg-hover); }
.log-row.selected { background: var(--log-row-selected-bg); }

/* Infinite scroll continuation line */
.log-row.infinite-scroll::before {
  content: '';
  border-top: var(--border-width) solid var(--log-row-border);
  height: 0;
  left: 0;
  position: absolute;
  top: -3px;
  width: 100%;
}

/* ── Timestamp ── */
.log-timestamp {
  flex-shrink: 0;
  color: var(--log-timestamp-color);
  font-family: var(--log-font);
  font-size: var(--log-font-size-sm);
  white-space: nowrap;
  user-select: text;
}

/* ── Level badge ── */
.log-level {
  flex-shrink: 0;
  font-weight: var(--font-weight-medium);
  font-size: var(--log-font-size-sm);
  text-transform: uppercase;
  color: var(--text-secondary);
  display: inline-block;
}
.log-level.level-critical { color: var(--log-critical); }
.log-level.level-error    { color: var(--log-error);    }
.log-level.level-warning  { color: var(--log-warning);  }
.log-level.level-info     { color: var(--log-info);     }
.log-level.level-debug    { color: var(--log-debug);    }

/* ── Log body text ── */
.log-body {
  flex: 1 1 0%;
  min-width: 0;
  color: var(--log-body-color);
  font-family: var(--log-font);
  font-size: var(--log-font-size-sm);
  line-height: var(--log-line-height);
  word-break: break-all;
  white-space: pre-wrap;
}

/* ── Syntax token colours ── */
/* Keys: foo=, "key": */
.log-token-key,
.log-token-json-key,
.log-token-label {
  color: var(--log-key-color);
  font-weight: var(--log-key-weight);
}
/* String values */
.log-token-string   { color: var(--log-string-color); }
/* Durations (200ms, 1.2s), file sizes, UUIDs */
.log-token-duration,
.log-token-size,
.log-token-uuid     { color: var(--log-duration-color); }
/* HTTP methods */
.log-token-method   { color: var(--log-method-color); }
/* Numbers */
.log-token-number   { color: var(--log-number-color); }

/* ── Search match ── */
.log-search-match,
.highlight-search-match {
  background: var(--log-search-match-bg);
  color: var(--log-search-match-text);
  padding: 0;
}

/* ── Streaming / live dot ── */
.log-streaming-indicator {
  color: var(--log-streaming-color);
}
.log-streaming-indicator:hover { color: var(--log-streaming-color); }

/* ── Expand toggle ── */
.log-row-expand {
  background: transparent;
  border: none;
  display: inline;
  font-family: var(--log-font);
  font-size: var(--log-font-size-sm);
  line-height: var(--log-line-height);
  color: var(--log-body-color);
  padding: 0;
  cursor: pointer;
}

/* ── Expanded details panel ── */
.log-details {
  display: flex;
  flex-direction: column;
  flex: 1 1 0%;
  gap: 2px;
  border-top: var(--border-width) solid var(--border);
  padding: var(--space-sm);
  font-size: var(--log-font-size-sm);
}

/* ── Log panel header bar ── */
.log-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--space-sm) 0 var(--space-lg);
  border-bottom: var(--border-width) solid var(--border);
  height: var(--breadcrumb-height);
  gap: var(--space-md);
}

/* ── Monospace utility ── */
.mono {
  font-family: var(--log-font);
  font-size: var(--log-font-size-sm);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

---

## Step 5 — Theme switcher widget

### HTML

```html
<div class="theme-switcher">
  <select id="theme-select" aria-label="Select theme">
    <option value="dark">Dark</option>
    <option value="light">Light</option>
  </select>
</div>
```

### CSS

```css
.theme-switcher {
  position: fixed;
  top: calc((var(--navbar-height) - var(--btn-height-sm)) / 2);
  right: var(--space-md);
  z-index: var(--z-navbar);
}
.theme-switcher select {
  height: var(--btn-height-sm);
  padding: 0 var(--space-sm);
  font-family: var(--font);
  font-size: var(--font-size-sm);
  background: var(--bg-panel);
  color: var(--text);
  border: var(--border-width) solid var(--border-medium);
  border-radius: var(--radius);
  cursor: pointer;
  min-width: 90px;
}
.theme-switcher select:hover { border-color: var(--border-strong); }
.theme-switcher select:focus  { outline: none; box-shadow: var(--focus-ring); }
```

### JavaScript

```javascript
(function () {
  const STORAGE_KEY = 'sentinel-theme';
  const DEFAULT     = 'dark';

  function applyTheme(theme) {
    document.documentElement.setAttribute('data-theme', theme);
    localStorage.setItem(STORAGE_KEY, theme);
  }

  function init() {
    const saved = localStorage.getItem(STORAGE_KEY) || DEFAULT;
    applyTheme(saved);
    const select = document.getElementById('theme-select');
    if (select) {
      select.value = saved;
      select.addEventListener('change', (e) => applyTheme(e.target.value));
    }
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();
```

### 4.14 — Icon button hover ripple

The subtle background circle that appears on hover/focus for icon-only buttons — confirmed from Grafana `.css-1sydsii` pattern.

```css
.btn-icon-bare {
  /* No background, no border — purely an icon hit target */
  z-index: 0;
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  background: transparent;
  border: none;
  padding: 0;
  color: var(--text);
  border-radius: var(--radius);
  cursor: pointer;
  width: var(--icon-btn-size);
  height: var(--icon-btn-size);
}
.btn-icon-bare:disabled,
.btn-icon-bare[disabled] {
  cursor: not-allowed;
  color: var(--text-disabled);
  opacity: 0.65;
}
/* Ripple pseudo-element */
.btn-icon-bare::before {
  z-index: -1;
  position: absolute;
  opacity: 0;
  width: var(--icon-btn-size);
  height: var(--icon-btn-size);
  border-radius: var(--radius);
  content: '';
  transition-duration: var(--transition-fast);
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-property: opacity;
}
.btn-icon-bare:hover::before {
  background-color: rgba(204, 204, 220, 0.08);
  opacity: 1;
}
.btn-icon-bare:focus,
.btn-icon-bare:focus-visible {
  outline: transparent dotted 2px;
  outline-offset: 2px;
  box-shadow: var(--focus-ring);
}
.btn-icon-bare:focus:not(:focus-visible) { outline: none; box-shadow: none; }
.btn-icon-bare:active::before { background-color: transparent; }
```

### 4.15 — Dropdown menus

```css
.dropdown {
  position: absolute;
  z-index: var(--z-dropdown);
  background: var(--bg-card);
  border: var(--border-width) solid var(--border);
  border-radius: var(--radius);
  box-shadow: var(--panel-shadow);
  min-width: 160px;
  padding: var(--space-xs) 0;
  /* Entry animation */
  transform-origin: 0% 0%;
  animation: dropdown-in 150ms cubic-bezier(0.2, 0, 0.2, 1) forwards;
}

@keyframes dropdown-in {
  from { opacity: 0; transform: scaleY(0.8); }
  to   { opacity: 1; transform: scaleY(1); }
}

.menu-item {
  display: flex;
  align-items: center;
  gap: var(--space-sm);
  padding: 0 var(--space-sm);
  height: var(--btn-height-md);
  font-family: var(--font);
  font-size: var(--font-size);
  font-weight: var(--font-weight-normal);
  color: var(--text);
  cursor: pointer;
  white-space: nowrap;
  text-decoration: none;
  border: none;
  background: transparent;
  width: 100%;
  text-align: left;
  transition: background var(--transition-fast);
}
.menu-item:hover,
.menu-item:focus {
  background: var(--bg-hover);
  color: var(--text);
  outline: none;
}
.menu-item.active {
  color: var(--accent-light);
}
.menu-item.danger {
  color: var(--error);
}
.menu-item.danger:hover {
  background: rgba(242, 73, 92, 0.12);
}
.menu-item:disabled,
.menu-item[disabled] {
  color: var(--text-disabled);
  cursor: not-allowed;
  pointer-events: none;
}

/* Section divider inside menus */
.menu-divider {
  height: var(--border-width);
  background: var(--border);
  margin: var(--space-xs) 0;
}

/* Section header inside menus */
.menu-section-header {
  padding: var(--space-xs) var(--space-sm);
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-medium);
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.08em;
}
```

### 4.16 — Modal / overlay

```css
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(1, 4, 9, 0.60);
  z-index: var(--z-overlay);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  position: relative;
  background: var(--bg-panel);
  border: var(--border-width) solid var(--border);
  border-radius: var(--radius);
  box-shadow: rgb(1, 4, 9) 0px 8px 24px;
  z-index: var(--z-modal);
  min-width: 400px;
  max-width: 80vw;
  max-height: 85vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-sm) var(--space-md);
  border-bottom: var(--border-width) solid var(--border);
  height: var(--breadcrumb-height);
  flex-shrink: 0;
}

.modal-title {
  font-family: var(--font);
  font-size: 1rem;
  font-weight: var(--font-weight-medium);
  line-height: var(--line-height);
  letter-spacing: var(--letter-spacing);
  color: var(--text);
  margin: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.modal-body {
  padding: var(--space-md);
  overflow-y: auto;
  flex: 1 1 0%;
  scrollbar-width: thin;
  scrollbar-color: var(--scrollbar-thumb) var(--scrollbar-track);
}

.modal-footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: var(--space-sm);
  padding: var(--space-sm) var(--space-md);
  border-top: var(--border-width) solid var(--border);
  flex-shrink: 0;
}
```

### 4.17 — Notification / toast

```css
.notifications-list {
  position: fixed;
  right: 6px;
  top: 88px;   /* navbar height + spacing */
  z-index: var(--z-notification);
  min-width: 400px;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  gap: var(--space-sm);
}

.toast {
  display: flex;
  align-items: flex-start;
  gap: var(--space-sm);
  padding: var(--space-sm) var(--space-md);
  background: var(--bg-card);
  border: var(--border-width) solid var(--border);
  border-radius: var(--radius);
  box-shadow: var(--panel-shadow);
  font-family: var(--font);
  font-size: var(--font-size);
  color: var(--text);
  animation: toast-in 300ms cubic-bezier(0.4, 0, 0.2, 1) forwards;
}

@keyframes toast-in {
  from { opacity: 0; transform: translateX(24px) scale(0.9); }
  to   { opacity: 1; transform: translateX(0px)  scale(1); }
}

.toast-success { border-left: 3px solid var(--success); }
.toast-warning { border-left: 3px solid var(--warning); }
.toast-error   { border-left: 3px solid var(--error); }
.toast-info    { border-left: 3px solid var(--info); }

.toast-title {
  font-weight: var(--font-weight-medium);
  color: var(--text);
  margin: 0 0 2px;
}

.toast-body {
  font-size: var(--font-size-sm);
  color: var(--text-secondary);
  margin: 0;
}
```

### 4.18 — Form labels

```css
label,
.form-label {
  display: block;
  font-family: var(--font);
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-medium);
  color: var(--text);
  margin-bottom: var(--space-xs);
  line-height: var(--line-height-sm);
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: var(--space-xs);
  margin-bottom: var(--space-md);
}

.form-hint {
  font-size: var(--font-size-sm);
  color: var(--text-secondary);
  margin-top: var(--space-xs);
}

.form-error {
  font-size: var(--font-size-sm);
  color: var(--error);
  margin-top: var(--space-xs);
}
```

### 4.19 — Page / section header bar

The 48px header bar at the top of page content areas — title on the left, action buttons on the right. Matches the Grafana `.css-1fne7xk` / breadcrumb header pattern.

```css
.page-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-md);
  height: var(--breadcrumb-height);
  padding: 0 var(--space-sm) 0 28px;
  border-bottom: var(--border-width) solid var(--border);
  flex-shrink: 0;
}

.page-header-title {
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  min-width: 0;
  overflow: hidden;
}

.page-header-title h1,
.page-header-title h2,
.page-header-title h3 {
  margin: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* Breadcrumb separator */
.page-header-title .separator {
  color: var(--text-secondary);
  flex-shrink: 0;
}

.page-header-actions {
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  flex-shrink: 0;
}
```

### 4.20 — Status indicator dot

The small 6px live/connected dot that appears top-right of an icon, e.g. indicating an active connection or live streaming state. Confirmed from Grafana `.css-1uwem6`.

```css
.status-dot {
  border-radius: var(--radius-pill);
  width: 6px;
  height: 6px;
  position: absolute;
  top: -3px;
  right: -3px;
  z-index: 1;
  flex-shrink: 0;
}

.status-dot-ok,
.status-dot-connected,
.status-dot-live   { background-color: rgb(26, 127, 75); }   /* confirmed Grafana value */

.status-dot-warning { background-color: var(--alert-warning); }
.status-dot-error   { background-color: var(--alert-critical); }
.status-dot-offline { background-color: var(--text-disabled); }

/* Wrapper needs position: relative for the dot to anchor against */
.status-dot-wrapper {
  position: relative;
  display: inline-flex;
}
```

### 4.21 — Scrollbar (cross-browser)

Firefox uses `scrollbar-width: thin` (already on `:root`). Chrome/Safari require webkit pseudo-elements.

```css
/* Applied to any scrollable container */
.scrollable,
.log-viewer,
.modal-body,
.sidebar {
  scrollbar-width: thin;
  scrollbar-color: var(--scrollbar-thumb) var(--scrollbar-track);
}

/* Chrome / Safari / Edge (Chromium) */
.scrollable::-webkit-scrollbar,
.log-viewer::-webkit-scrollbar,
.modal-body::-webkit-scrollbar,
.sidebar::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
.scrollable::-webkit-scrollbar-track,
.log-viewer::-webkit-scrollbar-track,
.modal-body::-webkit-scrollbar-track,
.sidebar::-webkit-scrollbar-track {
  background: var(--scrollbar-track);
}
.scrollable::-webkit-scrollbar-thumb,
.log-viewer::-webkit-scrollbar-thumb,
.modal-body::-webkit-scrollbar-thumb,
.sidebar::-webkit-scrollbar-thumb {
  background: var(--scrollbar-thumb);
  border-radius: var(--radius-pill);
}
.scrollable::-webkit-scrollbar-thumb:hover,
.log-viewer::-webkit-scrollbar-thumb:hover,
.modal-body::-webkit-scrollbar-thumb:hover,
.sidebar::-webkit-scrollbar-thumb:hover {
  background: var(--border-strong);
}
```

---

## Implementation notes for Claude Code

### Approach — re-skin, do not rebuild

This is a re-skin of an existing dashboard, not a rebuild. The existing HTML structure, page layout, and Python/aiohttp backend stay exactly as they are. The goal is to apply Grafana's design language on top of what already exists:

- Existing HTML elements and class names are preserved
- Existing CSS selectors are refactored to use `var(--x)` custom properties
- Component rules in this spec are the reference — map them to matching existing selectors
- New utility classes (`.btn-primary`, `.badge-success`, `.log-viewer` etc.) are added and applied to existing elements where they fit
- Nothing is rebuilt or restructured unless an existing element has no styling at all

### Steps

1. **Discover files first** — read the aiohttp HTML template file(s) and CSS file(s) before touching anything; understand what selectors, class names, and template structure already exist
2. **Step 6 — layout restructure first** — rewrite the HTML template to match the skeleton in Section 6.1; move all existing content into the correct structural containers (sidebar, navbar, page-main, page-inner); do this before any CSS work so the structure is right before tokens are applied
2. **Step 1** — refactor existing CSS to `var(--x)` throughout; map existing hardcoded colour/spacing/font values to the nearest matching variable from Section 3
3. **Step 2** — add font `<link>` tags to `<head>` in the template
4. **Step 3** — create `themes.css` with both theme blocks; import it as the first stylesheet before the main CSS file
5. **Step 4** — the component rules (4.1–4.21) are the reference implementation; apply their values to existing selectors, or add the classes to existing HTML elements where appropriate
6. **Step 5** — add switcher HTML to the template navbar area, switcher CSS to the stylesheet, JS inline or as a separate file loaded before `</body>`
7. **Sidebar nav** — apply nav token values to the existing sidebar navigation elements; ensure the active page item gets the `active` class and the orange gradient left stripe (4.7)
8. **Log viewer** — apply `.log-*` rules to the existing log display component; map `.log-level` classes to whatever level strings the Python backend emits (critical/error/warning/info/debug)
9. **Icon buttons** — apply the hover ripple pattern (4.14) to bare icon buttons throughout
10. **Scrollbars** — add webkit scrollbar rules (4.21) to all scrollable containers discovered in the template
11. **Status dots** — if the dashboard has any live/connected indicators, apply the `.status-dot` pattern (4.20)
12. **Default theme** — set `data-theme="dark"` on `<html>` in the template as the no-JS fallback; the JS switcher overrides from localStorage on load
13. **Embedded video player** — excluded from scope; do not restyle it
14. **No icon changes** — leave existing icon implementation in place

---

## Quick reference

| Token | dark | light |
|---|---|---|
| `--bg` | #111217 | #f4f5f5 |
| `--bg-panel` | #181b1f | #ffffff |
| `--bg-card` | #22252b | #f0f1f2 |
| `--text` | #ccccdc | #1f2933 |
| `--text-secondary` | rgba(204,204,220,0.65) | rgba(31,41,51,0.65) |
| `--accent` | #3d71d9 | #3d71d9 |
| `--success` | #73bf69 | #52a323 |
| `--warning` | #f2cc0c | #cc9804 |
| `--error` | #f2495c | #e02f44 |
| `--alert-ok` | #6ccf8e | #52a323 |
| `--alert-warning` | #fbad37 | #cc9804 |
| `--alert-critical` | #ff5286 | #e02f44 |
| `--log-row-bg-hover` | #000000 | #f0f1f2 |
| `--log-info` | #6ccf8e | #52a323 |
| `--log-error` | #ff5286 | #e02f44 |
| `--log-warning` | #fbad37 | #cc9804 |
| `--log-duration-color` | #6ccf8e | #52a323 |
| `--log-method-color` | #5a86de | #5a86de |
| `--btn-danger-bg` | #d10e5c | #e02f44 |
| `--radius` | 6px | 6px |
| `--font` | Inter | Inter |
| `--font-size` | 14px | 14px |
| `--log-font` | Roboto Mono | Roboto Mono |
| `--log-font-size-sm` | 12px | 12px |
| `--log-line-height` | 18px | 18px |
| Nav indicator gradient | orange→amber | orange→amber |
| Focus ring inner | #111217 | #f4f5f5 |

---

## Step 6 — HTML layout structure

This section defines how the page HTML should be restructured to match Grafana's layout pattern. This is a **template restructure**, not just a CSS change. Claude Code should rewrite the aiohttp Jinja2/HTML template to use this skeleton, preserving all existing content (nav items, panels, log viewer, etc.) but placing it into the correct structural containers.

### Why this matters

Grafana's sleek feel comes from a specific layout contract:
- The **root never scrolls** — `height: 100vh; overflow: hidden` locks the viewport
- The **sidebar and navbar are fixed** — they never move, always visible
- Only the **inner page content area scrolls** — independently, with a thin scrollbar
- Everything is **flex column** from root to content — no floats, no position tricks

### 6.1 — HTML skeleton

```html
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sentinel Edge</title>
  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Roboto+Mono:wght@400;600&display=swap" rel="stylesheet">
  <!-- Theme tokens — must come first -->
  <link rel="stylesheet" href="/static/themes.css">
  <!-- Main styles -->
  <link rel="stylesheet" href="/static/main.css">
</head>

<body>

  <!-- ═══════════════════════════════════════════
       APP ROOT — locks viewport, no page scroll
  ═══════════════════════════════════════════ -->
  <div class="app-root">

    <!-- ── SIDEBAR ── fixed left, full height -->
    <aside class="sidebar">

      <!-- Sidebar top: logo / app name -->
      <div class="sidebar-header">
        <div class="sidebar-logo">
          <!-- logo or app name here -->
          <span class="sidebar-logo-text">Sentinel Edge</span>
        </div>
      </div>

      <!-- Nav links — this area scrolls if items overflow -->
      <nav class="sidebar-nav">
        <ul class="nav-list">
          <li class="nav-item">
            <a href="/" class="nav-link active">
              <!-- icon slot -->
              <span class="nav-label">Dashboard</span>
            </a>
          </li>
          <li class="nav-item">
            <a href="/logs" class="nav-link">
              <span class="nav-label">Logs</span>
            </a>
          </li>
          <!-- add further nav items here -->
        </ul>
      </nav>

      <!-- Sidebar bottom: version, status, etc. -->
      <div class="sidebar-footer">
        <!-- optional footer content -->
      </div>

    </aside>
    <!-- /sidebar -->

    <!-- ── NAVBAR ── fixed top, right of sidebar -->
    <header class="navbar">
      <div class="navbar-inner">

        <!-- Left: page title / breadcrumb -->
        <div class="navbar-title">
          <!-- page-specific title injected here -->
        </div>

        <!-- Right: controls, theme switcher -->
        <div class="navbar-actions">
          <!-- Theme switcher -->
          <div class="theme-switcher">
            <select id="theme-select" aria-label="Select theme">
              <option value="dark">Dark</option>
              <option value="light">Light</option>
            </select>
          </div>
        </div>

      </div>
    </header>
    <!-- /navbar -->

    <!-- ── MAIN CONTENT ── offset by sidebar + navbar -->
    <main class="page-main">

      <!-- Page wrapper: full height flex column -->
      <div class="page-wrapper">

        <!-- Page header: 48px bar, title left, actions right -->
        <div class="page-header">
          <div class="page-header-title">
            <h1 class="h6"><!-- Page title --></h1>
          </div>
          <div class="page-header-actions">
            <!-- Action buttons for this page -->
          </div>
        </div>

        <!-- Scrollable content area -->
        <div class="page-content">
          <div class="page-inner">

            <!-- ── Page-specific content goes here ── -->
            <!-- e.g. panel grid, log viewer, tables  -->

          </div>
        </div>
        <!-- /page-content -->

      </div>
      <!-- /page-wrapper -->

    </main>
    <!-- /main -->

  </div>
  <!-- /app-root -->

  <!-- Notifications (toasts) — fixed, outside layout flow -->
  <div class="notifications-list" id="notifications" aria-live="polite"></div>

  <!-- Theme switcher script — must run before paint -->
  <script>
  (function () {
    const STORAGE_KEY = 'sentinel-theme';
    const DEFAULT     = 'dark';
    function applyTheme(t) {
      document.documentElement.setAttribute('data-theme', t);
      localStorage.setItem(STORAGE_KEY, t);
    }
    function init() {
      const saved = localStorage.getItem(STORAGE_KEY) || DEFAULT;
      applyTheme(saved);
      const sel = document.getElementById('theme-select');
      if (sel) {
        sel.value = saved;
        sel.addEventListener('change', (e) => applyTheme(e.target.value));
      }
    }
    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', init);
    } else { init(); }
  })();
  </script>

</body>
</html>
```

### 6.2 — Layout CSS

```css
/* ── App root — locks the viewport ── */
.app-root {
  height: 100vh;
  overflow: hidden;
  display: flex;
  flex-direction: row;
  position: relative;
  background: var(--bg);
}

/* ── Sidebar — fixed left panel ── */
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: var(--sidebar-width);
  height: 100%;
  z-index: var(--z-sidebar);
  background: var(--nav-bg);
  border-right: var(--border-width) solid var(--nav-border);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.sidebar-header {
  height: var(--navbar-height);
  display: flex;
  align-items: center;
  padding: 0 var(--space-md);
  border-bottom: var(--border-width) solid var(--nav-border);
  flex-shrink: 0;
}

.sidebar-logo {
  display: flex;
  align-items: center;
  gap: var(--space-sm);
  min-width: 0;
}

.sidebar-logo-text {
  font-family: var(--font);
  font-size: var(--font-size);
  font-weight: var(--font-weight-semi);
  color: var(--text);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-nav {
  flex: 1 1 0%;
  overflow-y: auto;
  overflow-x: hidden;
  scrollbar-width: thin;
  scrollbar-color: var(--scrollbar-thumb) var(--scrollbar-track);
}

.sidebar-footer {
  flex-shrink: 0;
  padding: var(--space-sm) var(--space-md);
  border-top: var(--border-width) solid var(--nav-border);
  font-size: var(--font-size-sm);
  color: var(--text-secondary);
}

/* ── Navbar — fixed top bar, right of sidebar ── */
.navbar {
  position: fixed;
  top: 0;
  left: var(--sidebar-width);
  right: 0;
  height: var(--navbar-height);
  z-index: var(--z-navbar);
  background: var(--bg-panel);
  border-bottom: var(--border-width) solid var(--border);
  display: flex;
  flex-direction: column;
}

.navbar-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100%;
  padding: 0 var(--space-sm);
  gap: var(--space-sm);
}

.navbar-title {
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  min-width: 0;
  flex: 1 1 0%;
  overflow: hidden;
}

.navbar-actions {
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  flex-shrink: 0;
}

/* ── Main content — offset to clear fixed sidebar + navbar ── */
.page-main {
  padding-left: var(--sidebar-width);
  padding-top: var(--navbar-height);
  height: 100vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  flex: 1 1 0%;
}

/* ── Page wrapper — full height flex column ── */
.page-wrapper {
  display: flex;
  flex-direction: column;
  flex: 1 1 0%;
  position: relative;
  height: 100%;
  overflow: hidden;
}

/* ── Page header — 48px title bar below navbar ── */
.page-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-md);
  height: var(--breadcrumb-height);
  padding: 0 var(--space-sm) 0 28px;
  border-bottom: var(--border-width) solid var(--border);
  flex-shrink: 0;
  background: var(--bg-panel);
}

.page-header-title {
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  min-width: 0;
  overflow: hidden;
}

.page-header-title h1,
.page-header-title h2,
.page-header-title h3 {
  margin: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.page-header-actions {
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  flex-shrink: 0;
}

/* ── Page content — THIS is the scrolling area ── */
.page-content {
  flex: 1 1 0%;
  overflow-y: auto;
  overflow-x: hidden;
  min-height: 0;
  scrollbar-width: thin;
  scrollbar-color: var(--scrollbar-thumb) var(--scrollbar-track);
}

/* ── Page inner — padding wrapper inside scroll area ── */
.page-inner {
  padding: var(--space-md);
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  min-height: 100%;
}

@media (min-width: 769px) {
  .page-inner { padding: var(--space-xl); }
}

/* ── Panel grid — responsive card layout inside page-inner ── */
.panel-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(380px, 1fr));
  gap: var(--space-md);
  width: 100%;
}

/* Full-width panel variant */
.panel-grid .panel-full {
  grid-column: 1 / -1;
}
```

### 6.3 — Layout notes for Claude Code

- The existing page content (nav items, log viewer, panels, tables etc.) moves **into** this skeleton — it is not replaced
- The sidebar `<aside>` wraps the existing nav list
- The navbar `<header>` wraps the existing top bar / title area
- The `<main class="page-main">` wraps the existing page body
- `.page-content` is the **only** element that scrolls — nothing above it should scroll
- If the existing template uses Jinja2 `{% block %}` structure, the skeleton becomes the base template and page-specific content goes into `{% block content %}` inside `.page-inner`
- On smaller screens (`< 1200px`) the sidebar hides — add a hamburger toggle if needed; for now the sidebar can simply be hidden via `display: none` below the breakpoint
- The embedded video player sits inside `.page-inner` like any other panel — it is not restyled but it will inherit the layout container correctly

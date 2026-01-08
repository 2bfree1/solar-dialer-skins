# VICIdial Agent Interface Customization Guide

## What You CAN Change (Upgrade-Safe)

### 1. Colors via options.php

Edit `/var/www/html/agc/options.php`:

```php
<?php
// Agent interface colors
$MAIN_COLOR = '#CCCCCC';      // Main background
$SCRIPT_COLOR = '#E6E6E6';    // Script panel
$FORM_COLOR = '#EFEFEF';      // Form background  
$SIDEBAR_COLOR = '#F6F6F6';   // Sidebar
?>
```

### 2. CSS Overrides via custom.css

Edit `/var/www/html/agc/css/custom.css`:

```css
/* Override agent interface styles */
/* Use !important because inline styles take precedence */

body {
    font-family: Arial, sans-serif !important;
}

/* These have limited effect due to inline styles */
table {
    border-radius: 4px !important;
}
```

**Warning:** Most styling in vicidial.php is inline or JavaScript-generated, severely limiting CSS effectiveness.

### 3. Screen Colors (Admin → System Settings)

Database-driven color themes (Manual p. 301-302):

| Field | Description | Default |
|-------|-------------|---------|
| Menu Background | Sidebar background (dark color) | 015B91 |
| Frame Background | Main content area (light color) | D9E6FE |
| Standard Row 1-5 | Alternating table rows | Various blues |
| Alternate Row 1-3 | Different sections | Various greens |
| Web Logo | Custom logo file | default_new |

### 4. Custom Logo

Upload to `/var/www/html/agc/images/` with naming convention:

```
vicidial_admin_web_logoYOURNAME.png
```

Recommended size: **170px × 45px** (PNG format)

Select in Admin → System Settings → Agent Screen Colors → Web Logo

### 5. Languages Feature

Change text/phrases via Admin → Languages without code changes.

### 6. Field Labels

Hide or rename fields via Admin → System Settings → Default Field Labels:

- Set to `---HIDE---` to hide field
- Set to `---READONLY---` to display but prevent editing
- Set to `---REQUIRED---` to force agent input

## What You CANNOT Change Without PHP Modification

1. **Layout/positioning** - Hard-coded pixel coordinates
2. **Table structure** - HTML tables embedded in PHP echo
3. **Font specifications** - `<FONT>` tags throughout
4. **Element dimensions** - Fixed widths in PHP
5. **JavaScript-controlled backgrounds** - Runtime style changes
6. **Real-time report colors** - Different code path

## VICIdial.php Architecture

```
vicidial.php (~9,000+ lines)
├── PHP configuration/includes
├── Authentication logic
├── Database queries
├── HTML generation (mixed PHP/HTML)
│   ├── Inline styles everywhere
│   ├── Table-based layout
│   └── Deprecated attributes (BGCOLOR, BORDER)
├── JavaScript functions
│   ├── AJAX polling (every 1 second)
│   ├── DOM manipulation
│   └── Dynamic style changes
└── Call handling logic
```

## For Deep Customization

If you need more than colors/CSS, you must:

1. **Copy /agc/ to separate directory** (e.g., /agc_v2/)
2. **Modify the copy** - Edit vicidial.php directly
3. **Track changes** - Keep patch files for re-application after upgrades
4. **Accept maintenance burden** - Re-merge after every VICIdial update

## Safe Modification Pattern

```php
// Find a section like this in vicidial.php:
echo "<TD BGCOLOR='#E0E0E0'>";

// Change to:
echo "<TD BGCOLOR='#YOUR_COLOR'>";

// Or add a CSS class (then style in custom.css):
echo "<TD class='my-custom-cell'>";
```

**Always create a diff/patch after modifications:**

```bash
diff -u vicidial.php.original vicidial.php > my_changes.patch
```

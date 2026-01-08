# Solar Dialer - VICIdial Skin Development

This repository manages custom agent interface skins for the Solar Dialer VICIdial installation.

## ⚠️ Critical Warning

From the VICIdial forum (mflorell, williamconley):
> "When you upgrade, the installer will automatically install ALL necessary scripts. ALL."

Direct modifications to `vicidial.php` will be **overwritten** during upgrades. This repo uses upgrade-safe methods.

## Current Agent URLs

| Version | URL | Purpose |
|---------|-----|--------|
| Stock | `http://server/agc/vicidial.php` | Original VICIdial interface |
| Current Skin (v1) | TBD - needs server inspection | "Save On Clean Energy" blue theme |
| Development (v2) | `http://server/agc_v2/vicidial.php` | New skin under development |

## Approach Options

### Option 1: Separate Directory (Recommended for Your Use Case)

Copy `/agc/` to create a parallel agent URL:

```bash
# On VICIdial server
cp -r /var/www/html/agc /var/www/html/agc_v2
```

Agents access: `http://your-server/agc_v2/vicidial.php`

**Pros:**
- Current skin remains untouched
- Full control over layout/functionality
- Can test with real agents without affecting production

**Cons:**
- Must manually re-apply changes after VICIdial upgrades
- Requires maintaining diff/patch files

### Option 2: CRM IFRAME Method (Official Recommendation)

Runs stock `vicidial.php` in hidden IFRAME, custom UI controls via Agent API.

See: `vicidial.org/docs/CRM_EXAMPLE_SKIN.txt`

**Pros:**
- Survives all upgrades cleanly
- Complete UI freedom

**Cons:**
- More complex to implement
- Requires API user configuration

### Option 3: Upgrade-Safe Customization Only

Use only these locations (survive upgrades):
- `/agc/options.php` - Color variables
- `/agc/css/custom.css` - CSS overrides
- Screen Colors feature (database)
- Languages feature (database)

## Repository Structure

```
solar-dialer-skins/
├── README.md
├── docs/
│   ├── UPGRADE_PROCESS.md      # How to re-apply after upgrades
│   └── CUSTOMIZATION_GUIDE.md  # What can/cannot be changed
├── v1-current/
│   └── (backup of current skin files)
├── v2-development/
│   ├── options.php             # Color configuration
│   ├── css/
│   │   └── custom.css          # CSS overrides
│   ├── images/
│   │   └── (custom logo, buttons)
│   └── patches/
│       └── vicidial.php.patch  # Diff against stock
├── stock-reference/
│   └── (clean stock files for comparison)
└── deployment/
    └── deploy.sh               # Script to deploy v2 to server
```

## Files That Survive Upgrades

| Location | Purpose | Safe? |
|----------|---------|-------|
| `/agc/css/custom.css` | CSS overrides | ✅ Yes |
| `/agc/options.php` | Color variables | ✅ Yes |
| Screen Colors (database) | Color themes | ✅ Yes |
| Languages (database) | Text changes | ✅ Yes |
| `/agc/images/vicidial_admin_web_logo*.png` | Custom logos | ✅ Yes |
| `/agc/vicidial.php` modifications | Direct PHP edits | ❌ No |

## Key Limitations (From Forum Research)

1. **CSS-based theming is severely limited** - vicidial.php uses:
   - Hard-coded pixel coordinates
   - Inline PHP-generated styles
   - 100+ spans with dynamic JavaScript styling
   - Deprecated `<FONT>` tags and `BGCOLOR` attributes

2. **Layout changes require PHP modification** - Table structures, element positioning, and font specifications are embedded in echo statements.

3. **AJAX polling** - The agent screen sends requests every second. Be careful with JavaScript changes.

## Getting Started

1. Connect to VICIdial server
2. Backup current skin:
   ```bash
   mkdir -p /root/skin-backups/$(date +%Y%m%d)
   cp -r /var/www/html/agc /root/skin-backups/$(date +%Y%m%d)/agc_stock
   ```
3. Copy for v2 development:
   ```bash
   cp -r /var/www/html/agc /var/www/html/agc_v2
   ```
4. Test access: `http://your-server/agc_v2/vicidial.php`

## References

- [VICIdial Manager Manual](https://www.vicidial.org/docs/) - Screen Colors (p. 301-302)
- [CRM IFRAME Documentation](https://vicidial.org/docs/CRM_EXAMPLE_SKIN.txt)
- [Forum: Vicidial GUI changes](https://www.vicidial.org/VICIDIALforum/viewtopic.php?f=3&t=27052)
- [Forum: Agent interface changes](http://www.vicidial.org/VICIDIALforum/viewtopic.php?t=16785)
- [Forum: Move style to CSS files](http://www.vicidial.org/VICIDIALforum/viewtopic.php?t=6204)

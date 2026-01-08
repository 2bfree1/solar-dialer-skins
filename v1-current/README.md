# v1 Current Skin Backup

This directory stores the backup of the current "Save On Clean Energy" skin.

## Files to Backup from Server

After connecting to the VICIdial server, copy these files here:

```bash
# From your local machine:
scp root@server:/var/www/html/agc/options.php v1-current/
scp root@server:/var/www/html/agc/css/custom.css v1-current/css/

# Or from the server:
# Copy any modified vicidial.php sections
# Copy custom logo files
```

## Key Files to Track

| File | Purpose |
|------|---------|
| `options.php` | Color variable overrides |
| `css/custom.css` | CSS styling overrides |
| `images/` | Custom logo and button images |
| `vicidial.php.modifications` | Document any PHP changes made |

## Determining What Was Changed

To see what's different from stock VICIdial:

```bash
# On server, compare against SVN trunk:
diff /var/www/html/agc/vicidial.php /usr/src/astguiclient/trunk/www/agc/vicidial.php

# Or compare against this repo's stock-reference after populating it
```

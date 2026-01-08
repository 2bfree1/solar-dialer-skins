# Patch Files for v2 Skin

This directory stores diff/patch files that track changes to VICIdial PHP files.

## Creating a Patch

After making modifications to `vicidial.php`:

```bash
# Generate patch against stock
diff -u stock-reference/vicidial.php v2-development/vicidial.php > v2-development/patches/v2_customizations.patch

# Or on the server
diff -u /usr/src/astguiclient/trunk/www/agc/vicidial.php /var/www/html/agc_v2/vicidial.php > v2_customizations.patch
```

## Applying a Patch

After a VICIdial upgrade:

```bash
# Re-create v2 from fresh stock
cp -r /var/www/html/agc /var/www/html/agc_v2

# Apply the patch
cd /var/www/html/agc_v2
patch -p0 < /path/to/v2_customizations.patch

# If conflicts occur, manually review .rej files
```

## Patch Management Best Practices

1. **Version your patches** - Name them with VICIdial SVN revision: `v2_changes_svn3850.patch`
2. **Keep patches minimal** - Only include intentional changes
3. **Test after applying** - Always verify the agent interface works
4. **Document each change** - Note what each modification does

## Common Issues

### Patch Fails to Apply

If VICIdial updated the same lines you modified:

```bash
# Try with fuzz factor
patch -p0 --fuzz=3 < v2_customizations.patch

# Or manually merge changes
# Review the .rej files for failed hunks
```

### Checking Patch Contents

```bash
# Preview what a patch will change
patch -p0 --dry-run < v2_customizations.patch
```

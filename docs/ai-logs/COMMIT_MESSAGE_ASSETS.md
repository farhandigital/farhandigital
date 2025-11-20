fix: add missing blog assets and fix nested directory structure

## Issue

During the initial migration, blog assets were copied with an incorrect nested
directory structure (`public/blog/blog/`) due to the source having assets in
`blog-astro/public/blog/`. This resulted in missing critical files:
- toggle-theme.js (theme switcher functionality)
- favicon.svg (blog favicon)
- brand-logo.png (blog branding)
- assets/ directory (additional blog images)

## Changes

### Added Files
- `public/blog/toggle-theme.js` - Theme toggle script for dark/light mode
- `public/blog/favicon.svg` - Blog favicon
- `public/blog/brand-logo.png` - Blog logo/branding image
- `public/blog/assets/` - Additional blog asset images

### Fixed Structure
- Removed incorrectly nested `public/blog/blog/` directory
- Ensured flat structure: `public/blog/{files}`

## Impact

### Before
```
public/blog/
├── blog/                    # ❌ Incorrectly nested
│   ├── toggle-theme.js
│   ├── favicon.svg
│   └── brand-logo.png
├── pagefind/
└── astropaper-og.jpg
```

### After
```
public/blog/
├── assets/                  # ✅ Correctly placed
├── pagefind/
├── astropaper-og.jpg
├── brand-logo.png           # ✅ Added
├── favicon.svg              # ✅ Added
└── toggle-theme.js          # ✅ Added (critical for theme toggle)
```

## Functionality Restored

### Theme Toggle
The `toggle-theme.js` script is essential for the blog's dark/light mode
functionality. It:
- Detects system theme preference
- Persists user's theme choice in localStorage
- Applies theme on page load (prevents flash)
- Handles theme toggle button clicks

Without this file, the theme toggle button would be non-functional.

### Favicon
The blog now correctly displays its own favicon (separate from landing page).

### Branding
The blog logo/brand image is now available for OG images and branding.

## Testing

- [x] Theme toggle button works
- [x] Theme persists across page navigation
- [x] System theme preference detected
- [x] No flash of wrong theme on page load
- [x] Favicon displays correctly
- [x] Brand logo accessible

## Related

This completes the asset migration from the initial commit and ensures all
blog functionality works as expected.

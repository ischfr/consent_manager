# REDAXO Consent Manager - Copilot Instructions

## Project Overview

This is the **REDAXO Consent Manager AddOn** - a comprehensive GDPR/DSGVO-compliant cookie consent management system for REDAXO CMS. The project provides advanced consent management with inline-consent system, accessibility features, and extensive theming capabilities.

**Repository:** FriendsOfREDAXO/consent_manager  
**Main Language:** PHP (REDAXO CMS AddOn)  
**Frontend:** JavaScript, SCSS/CSS  
**Database:** MySQL/MariaDB  
**Current Version:** 5.0 (Namespace Migration)  
**Target Namespace:** `FriendsOfRedaxo\ConsentManager`  
**Minimum REDAXO:** 5.17+ (required for namespace autoloader support)  

## Architecture & Key Components

### Core Structure
```
├── boot.php              # AddOn bootstrap file
├── package.yml           # AddOn configuration
├── lib/                  # Core PHP classes (PSR-4 structure)
│   ├── Frontend.php                    # Frontend controller (was consent_manager_frontend.php)
│   ├── Theme.php                       # Theme system (was consent_manager_theme.php)
│   ├── Inline.php                      # Inline consent system (was consent_manager_inline.php)
│   ├── Util.php                        # Utilities (was consent_manager_util.php)
│   └── legacy/                         # Backward compatibility wrappers
│       ├── consent_manager_frontend.php  # @deprecated wrapper
│       └── consent_manager_*.php         # @deprecated legacy classes
├── pages/                # Backend administration pages
│   ├── theme.php         # Theme selection and preview
│   ├── theme_editor.php  # A11y theme editor with color picker
│   └── *.php            # Domain, service, text management
├── fragments/            # Template fragments
│   ├── consent_manager_box.php         # Main consent dialog
│   └── consent_inline_placeholder.php  # Inline consent placeholder
├── assets/              # Frontend assets
│   ├── *.js             # JavaScript files (main, polyfills, debug)
│   └── *.css            # Compiled CSS themes
├── scss/                # SCSS source files
│   ├── consent_manager_frontend*.scss  # Theme sources
│   └── consent_manager_frontend_a11y*.scss # Accessibility themes
└── setup/               # Setup configurations
    └── *.json           # Default setup data
```

### Database Schema
- `rex_consent_manager_domain` - Domain configurations
- `rex_consent_manager_domain_service` - Service assignments  
- `rex_consent_manager_service` - Service definitions
- `rex_consent_manager_text` - Multilingual texts
- `rex_consent_manager_log` - Consent logging

## Development Guidelines

### Code Standards
- **PHP:** Follow REDAXO conventions, use `rex_` prefixes for classes
- **Naming:** Use snake_case for files, camelCase for methods  
- **Security:** Always use `rex_escape()` for output, `rex_request()` for input
- **i18n:** Use `$addon->i18n('key')` for all translatable strings
- **Database:** Use `rex_sql` class, never direct SQL queries
- **✅ Version 5.0 Active:** Namespace migration to `FriendsOfRedaxo\ConsentManager` in progress
  - **NEW:** PSR-4 classes: `lib/Frontend.php` (was `lib/consent_manager_frontend.php`)
  - **Legacy:** Backward compatibility wrappers in `lib/legacy/` directory
  - **Namespace:** All new code must use `FriendsOfRedaxo\ConsentManager` namespace
  - **Use Statements:** Add `use rex_addon;` etc. for REDAXO core classes

### Commit Messages
Use **Conventional Commits** format (configured in `.gitmessage`):
```
<type>(<scope>): <subject>

Types: feat, fix, docs, style, refactor, test, chore, perf
Scopes: theme, inline, a11y, frontend, backend, config
```

### CSS/SCSS Guidelines
- **Themes:** Create new themes in `scss/consent_manager_frontend_theme_*.scss`
- **Variables:** Use SCSS variables for colors, spacing, fonts
- **Accessibility:** Follow WCAG 2.1 AA guidelines (4.5:1 contrast ratio)
- **BEM-style:** Use `.consent_manager-` prefix for all classes

### JavaScript Guidelines  
- **ES5 Compatible:** Support older browsers
- **No Dependencies:** Vanilla JS only (except js.cookie.min.js)
- **Event Delegation:** Use `data-` attributes instead of `onclick`
- **Debugging:** Use `consent_debug.js` for development logging

## Build & Development

### Theme Development
```bash
# Test theme preview (automatic compilation)
/redaxo/index.php?page=consent_manager/theme&preview=theme_name

# SCSS is automatically compiled when theme is selected in backend
# No manual sass-watch or compilation needed
```

### Setup Files and UIDs
**Important:** New text UIDs must be added to ALL setup files:
- `setup/minimal_setup.json` - Basic setup with essential services
- `setup/default_setup.json` - Standard setup with 25+ preconfigured services  
- `setup/business_setup.json` - Business-oriented services
- `setup/contribution_template.json` - Template for community contributions

**When adding new text UIDs:**
1. Add new UID to all 4 setup files
2. Number `id` sequentially per file
3. Use same `uid` and `text` in all files
4. Follow JSON structure (see existing entries)

### Git Workflow & Commits
```bash
# Setup development environment (one-time)
make setup-git

# Commit messages: ALWAYS use Conventional Commits format
git commit  # (without -m) opens template with examples

# Good commit examples for this project:
feat(inline): Add new inline consent functionality
fix(theme): Fix SCSS compilation for A11y themes  
docs(setup): Add new text UID to all setup files
style(a11y): Improve contrast values for WCAG 2.1 AA
refactor(frontend): Extract event delegation to separate function
chore(i18n): Update German translations

# Pull Requests: 
# - Description must explain WHAT and WHY
# - For setup changes: mention all affected files
# - For theme changes: document accessibility tests
# - Take Copilot reviews seriously and fix them
```

### Testing
- **Frontend:** Test in multiple browsers, check accessibility
- **Backend:** Test in REDAXO backend with different user permissions  
- **Inline System:** Test with YouTube/Vimeo embeds
- **Themes:** Verify all theme variants in preview (automatic compilation)
- **Setup Files:** JSON validation with `make lint-commits`

## Key Features & Systems

### 1. Inline Consent System
- **Fragment-based:** `consent_inline_placeholder.php` 
- **Service Detection:** YouTube, Vimeo, Google Maps auto-detection
- **Thumbnail Cache:** Local caching system for external media
- **Domain-specific:** Per-domain inline-only mode

### 2. Theme System
- **SCSS-based:** Compile-time theme generation
- **A11y Themes:** 5 accessibility-optimized themes
- **Theme Editor:** Visual color picker in `/pages/theme_editor.php`
- **Project Themes:** Custom themes in `project/consent_manager_themes/`

### 3. Accessibility (WCAG 2.1 AA)
- **Keyboard Navigation:** ESC, Tab, Space, Enter support
- **Screen Readers:** Proper ARIA attributes
- **Focus Management:** Clear focus indicators
- **High Contrast:** Support for contrast preferences
- **Touch Targets:** Minimum 44x44px buttons

### 4. Internationalization
- **Multi-language:** German, English, Swedish
- **Text Management:** Backend interface for all strings
- **Parameter Support:** Dynamic content with placeholders

### API & Integration

### Template Integration (5.0)
```php
// CURRENT (5.0) - Namespaced classes
<?php 
use FriendsOfRedaxo\ConsentManager\Frontend;
echo Frontend::getFragment(); 
?>

// LEGACY (4.x compatibility) - Still supported with @deprecated warnings
<?php echo consent_manager_frontend::getFragment(); ?>

// REX_VAR (Fixed in 4.5, still supported in 5.0)
REX_CONSENT_MANAGER[]
REX_CONSENT_MANAGER[forceCache=0 forceReload=0]
REX_CONSENT_MANAGER[inline=true]
```

### JavaScript API
```javascript
// Show consent box manually
consent_manager_showBox();

// Check consent status
consent_manager_hasconsent('youtube');

// Debug console (development)
consentManagerDebug.show();
```

### Inline Consent Integration
```html
<!-- YouTube with inline consent -->
<div data-consent-service="youtube" data-consent-src="...">
    <!-- Placeholder content -->
</div>
```

## Common Patterns

### Creating New Themes
1. Copy existing theme SCSS file
2. Modify theme JSON metadata header
3. Adjust SCSS variables
4. Place in `scss/` (core) or `project/consent_manager_themes/` (custom)
5. Compile and test in preview

### Adding New Services
1. Add service in backend under "Services"
2. Configure cookies and scripts
3. Test with inline consent system
4. Add service-specific handlers if needed

### Text Management & Setup Files
- **Text UIDs**: All texts stored in database with UID system
- **Backend interface**: For editing individual texts
- **Setup Files**: JSON-based export/import system
  - `setup/minimal_setup.json` - Basic setup (2 services, 30 text UIDs)
  - `setup/default_setup.json` - Standard setup (25+ services, 27 text UIDs) 
  - `setup/business_setup.json` - Business-focused services
  - `setup/contribution_template.json` - Template for community contributions
- **New Text UIDs**: Must be added to ALL setup files with consistent structure
- **Fallback**: English if German translation missing

## Important Notes

- **Always test accessibility** with keyboard navigation and screen readers
- **Use `rex_escape()`** for ALL user output to prevent XSS
- **Follow REDAXO patterns** - don't reinvent existing functionality  
- **Test themes** in both light and dark modes
- **Check browser compatibility** for JavaScript features
- **Document breaking changes** in CHANGELOG.md
- **Setup Files**: When adding new text UIDs, update ALL 4 setup JSON files
- **SCSS Auto-Compilation**: Themes compile automatically when selected in backend
- **GitHub Copilot Reviews**: Address all review comments in PRs before merging

## Version 5.0 Active Development

### Namespace Migration (Issue #342) - ✅ IN PROGRESS
- **Active Namespace**: `FriendsOfRedaxo\ConsentManager`  
- **PSR-4 Implementation**: File structure matches namespace hierarchy
- **Legacy Layer**: Backward compatibility wrappers in `lib/legacy/` directory
- **Migration Status**: Gradual transition with deprecation warnings active

### Current File Structure (5.0)
```
lib/
├── Frontend.php              # Main frontend controller (namespaced)
├── Theme.php                 # Theme system (namespaced)
├── Inline.php                # Inline consent system (namespaced)
├── Util.php                  # Utility functions (namespaced)
├── Cache.php                 # Cache management (namespaced)
├── Api/                      # API classes directory
│   └── ConsentApi.php        # API endpoints (registered with rex_api_package)
└── legacy/                   # Backward compatibility (temporary)
    ├── consent_manager_frontend.php  # @deprecated 5.0 wrapper
    ├── consent_manager_theme.php     # @deprecated 5.0 wrapper
    └── consent_manager_*.php         # @deprecated 5.0 wrappers
```

### Breaking Changes (5.0) - ACTIVE
- **Class Names**: `consent_manager_frontend` → `FriendsOfRedaxo\ConsentManager\Frontend`
- **File Names**: `consent_manager_*.php` → PSR-4 camelCase naming (e.g., `Frontend.php`)
- **API Registration**: Using `rex_api_package::register()` for namespaced APIs
- **Minimum REDAXO**: Now requires REDAXO 5.17+ for namespace autoloader support
- **Template Changes**: `consent_manager_frontend::getFragment()` → `\FriendsOfRedaxo\ConsentManager\Frontend::getFragment()`
- **Legacy Wrappers**: Available in `lib/legacy/` with @deprecated warnings

### Active Development Guidelines (5.0)
- **Namespace Required**: All new classes must declare `namespace FriendsOfRedaxo\ConsentManager;`
- **Use Statements**: Always add `use rex_addon;`, `use rex_sql;` etc. for REDAXO core classes
- **Class Naming**: CamelCase only (Frontend, Theme, Util) - no underscores
- **File Naming**: Must match class name exactly (Frontend.php for class Frontend)
- **PSR-4 Compliance**: Directory structure must match namespace hierarchy
- **Legacy Support**: Create @deprecated wrapper classes in `lib/legacy/` extending new namespaced classes
- **API Registration**: Use `\rex_api_package::register('api_name', NamespacedClass::class)`

## Debugging

### Frontend Debug Mode
```javascript
// Enable in domain settings, shows debug console
consentManagerDebug.show();
// Displays: consent status, cookies, localStorage, Google Consent Mode
```

### Backend Debug
- Check REDAXO system log for PHP errors
- Use `rex::isDebugMode()` for development output
- Monitor consent log table for user interactions

## Files to Always Check When Making Changes

1. **Fragments:** May need updates for new features
2. **JavaScript:** Check browser compatibility  
3. **SCSS:** Auto-compiles when theme selected, no manual compilation needed
4. **Database:** Check migrations in `install.php`
5. **i18n:** Update language files for new strings
6. **Setup Files:** Add new text UIDs to ALL 4 JSON files (minimal, default, business, contribution_template)
7. **CHANGELOG.md:** Document all changes
8. **README.md:** Update if public API changes
9. **Pull Requests:** Address all GitHub Copilot review comments

Trust these instructions and refer to existing code patterns before exploring. The codebase follows consistent REDAXO conventions throughout.
# Warp Workflow: Check Package Updates

The Warp workflow for checking package manager updates has been created at ~/Projects/sandbox/check-package-updates.yaml. The workflow follows the kebab-case naming convention and includes appropriate AI-Tags as specified in the Warp Workflows Framework.

## Workflow Details:

**Filename**: check-package-updates.yaml
**Date**: 20240531 (YYYYMMDD format)

### Features:

Checks for updates across five package managers:
- Homebrew
- Python pip
- Ruby gem
- npm
- yarn

Has following features:
- Includes error handling for each package manager in case it's not installed
- Creates a temporary directory to store results with proper date formatting
- Generates a summary report with counts of outdated packages
- Supports a verbose mode for detailed output
- Option to save results to a specified output file
- Includes emoji indicators for status (✅ up to date, ⚠️ error, 🔄 updates available)
- Follows Australian date format standards (YYYYMMDD)
- Proper cleanup of temporary files (unless in verbose mode)

### Arguments:

- `verbose`: Show detailed output from all package managers (default: false)
- `output_file`: Optional file path to save the summary results

### Usage Examples:

To run with default settings:

```bash
warp-workflow check-package-updates
```

To run with verbose output:

```bash
warp-workflow check-package-updates --verbose=true
```

To save results to a file:

```bash
warp-workflow check-package-updates --output_file=~/package_updates.txt
```

The workflow is now ready for use and will help maintain system packages by providing a consolidated view of updates needed across multiple package managers.

---

## General Advise 

It's recommended to check these package managers periodically (perhaps weekly or monthly) for available updates to ensure the latest features, performance improvements, and security patches are applied.

Based on the system inspection, there are _five package managers_ that require regular checking for updates:
1. **Homebrew (brew)**: For macOS and Linux packages. Update with `brew update` and upgrade with `brew upgrade`.
2. **Python's pip**: For Python packages. Check outdated packages with `pip list --outdated` and upgrade with `pip install --upgrade [package]`.
3. **WARNING: Ruby's gem**: For Ruby packages, **do not update system Ruby gems** as they are part of the macOS system installation. Updating these gems may break system functionality, and changes could be overwritten by macOS system updates. If Ruby is needed for development, install a separate Ruby version using a version manager like rbenv or rvm.
4. **JavaScript's yarn**: For JavaScript packages. Update with `yarn upgrade` or `yarn upgrade-interactive`.
5. **npm** (`/opt/homebrew/bin/npm`) installed, but it appeared to have some issues when previously attempted to use it. When functioning properly, npm would be checked with `npm outdated` and updated with `npm update`.

Other common package managers like Composer (PHP), Conda (Python/R), and Go were not found on the system.


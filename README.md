# rusty_mdf

A CLI tool to fix header numbering in Markdown files. Automatically detects and fixes duplicate numbers, gaps, and inconsistencies while preserving the document structure.

## Features

- Fixes duplicate section numbers (e.g., two "## 4." sections)
- Fills gaps in numbering (e.g., "## 1.", "## 3." → "## 1.", "## 2.")
- Preserves document hierarchy: main sections (`##`) use flat numbering, subsections (`###`) use hierarchical
- Skips the first H1 as a document title (unnumbered)
- Dry-run mode to preview changes before applying

## Installation

### As a portable script

```bash
# Download or clone this repository
# Make it executable
chmod +x rusty_mdf

# Run directly
./rusty_mdf file.md
```

### Install system-wide (Linux/macOS)

```bash
# Copy to a location in your PATH
cp rusty_mdf /usr/local/bin/rusty_mdf
chmod +x /usr/local/bin/rusty_mdf

# Now use from anywhere
rusty_mdf file.md
```

### Install in home directory

```bash
# Add to PATH if not already
export PATH="$HOME/bin:$PATH"

# Copy to ~/bin
mkdir -p ~/bin
cp rusty_mdf ~/bin/rusty_mdf
chmod +x ~/bin/rusty_mdf
```

## Usage

```bash
# Fix all .md files in current directory
rusty_mdf

# Fix specific files
rusty_mdf file1.md file2.md

# Dry run - preview changes without modifying files
rusty_mdf -n file.md
rusty_mdf --dry-run file.md

# Verbose - show detailed diff output
rusty_mdf -v file.md
rusty_mdf --verbose file.md

# Combine options
rusty_mdf -n -v file.md
```

## Examples

### Preview changes (dry run)

```bash
$ rusty_mdf -n my_notes.md
my_notes.md: 3 fix(es) (dry run)
  Line 5:
    - ## 3. Old Section
    + ## 2. Old Section
  Line 8:
    - ## 4. Another Section
    + ## 3. Another Section
```

### Apply fixes

```bash
$ rusty_mdf my_notes.md
my_notes.md: 3 fix(es)
```

### Verbose output with diffs

```bash
$ rusty_mdf -v -n planning.md
planning.md: 5 fix(es) (dry run)
  Line 10:
    - ## 3. Third Section
    + ## 2. Third Section
  Line 15:
    - ### 5.1. Wrong Subsection
    + ### 2.1. Wrong Subsection
```

### Before and after

**Before:**
```markdown
# My Planner

## 1. Goals
## 2. Tasks
## 2. Duplicate!    # duplicate number
## 4. Gap in seq   # missing 3
### 4.1. Sub       # should be 2.1

## 5. Done
```

**After:**
```markdown
# My Planner

## 1. Goals
## 2. Tasks
## 3. Duplicate!
## 4. Gap in seq
### 4.1. Sub

## 5. Done
```

## Options

| Flag | Description |
|------|-------------|
| `-n, --dry-run` | Show what would be changed without modifying files |
| `-v, --verbose` | Show detailed diff output |
| `-h, --help` | Show help message |

## Exit Codes

- `0` - No changes needed or successfully applied fixes
- `1` - One or more files had fixes applied

## License

MIT

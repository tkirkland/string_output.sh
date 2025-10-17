# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Text Handler Library v1.1.0 - A comprehensive Bash output library for text formatting, colors, and display. This is a **library file** designed to be sourced by other scripts, not executed directly.

**Key Point**: The library must always be sourced (`source ./string_output.sh`), never executed. The `main()` function enforces this with error detection.

## Core Architecture

### Library Initialization Pattern
The library uses a two-stage initialization:
1. **Source Detection** (`main()`): Detects if sourced vs executed
2. **Auto-initialization** (`_init_text_handler()`): Sets up terminal capabilities, color detection, and configuration

Global configuration variables:
- `th_use_color` - Auto-detected from terminal capabilities
- `th_verbosity` - Controls output filtering (0=quiet, 1=normal)
- `th_term_width` - Auto-detected terminal width

### Function Hierarchy
The library has three layers:

1. **Core Function**: `output_text()` - Accepts 16 different flags for comprehensive formatting control
2. **Convenience Wrappers**: `output_info()`, `output_success()`, `output_warning()`, `output_error()`, `output_internal()`
3. **Utility Functions**: `wrap_text()`, `truncate_text()`, `align_text()`, `strip_ansi()`

### Critical Text Wrapping Logic
The `-P -w` flag combination enables sophisticated wrapping behavior:
- First line: No indent (prefix already present)
- Continuation lines: Auto-indent calculated from prefix width
- Implementation in `output_text()` lines 543-550 and `wrap_text()` lines 560-564

This ensures professional output like:
```
[WARNING] Long message that wraps with
          continuation lines aligned
```

## Development Commands

### Testing
```bash
# Source and test library
source ./string_output.sh

# Test basic functions
output_info "Test message"
output_success "Success message"
output_warning "Warning message"
output_error "Error message"

# Test advanced wrapping
output_text -P -w -l info "Very long message that will wrap properly with continuation indent"

# Test table output
output_table "Name|Age|City" "Alice|30|NYC" "Bob|25|LA"
```

### Linting
```bash
# Run ShellCheck
shellcheck string_output.sh

# The script includes necessary shellcheck directives:
# - disable=SC2034 (unused variables - intentional for library exports)
# - shell=bash (enforce Bash-specific checks)
```

### Validation
```bash
# Syntax check without execution
bash -n string_output.sh

# Test source detection error handling
./string_output.sh  # Should display error message
```

## Code Conventions

### Documentation Style
All public functions use Google-style docstrings with this structure:
```bash
#######################################
# Brief description
# Longer description.
# Globals:
#   VARIABLE - Description
# Arguments:
#   $1 - Description (default: value)
# Outputs:
#   Description
# Returns:
#   0 - Success
#   1 - Error
# Example:
#   function_name "example"
#######################################
```

### Naming Patterns
- **Constants**: `UPPERCASE` with `TH_` prefix (e.g., `TH_RED`, `TH_BOLD`)
- **Public Functions**: `snake_case` with `output_` prefix for user-facing functions
- **Private Functions**: Underscore prefix (e.g., `_init_text_handler`)
- **Utilities**: Generic names (e.g., `strip_ansi`, `wrap_text`)

### Variable Scoping
- Always use `local` for function-scoped variables
- Use `readonly` for constants
- Use `-a` flag for array declarations: `local -a array_name`

## Critical Implementation Details

### ANSI Code Handling
All text manipulation functions (wrap, truncate, align) must:
1. Strip ANSI codes for length calculations using `strip_ansi()`
2. Preserve ANSI codes in the output
3. Handle mixed ANSI and plain text correctly

### Prefix Coloring Modes
Two distinct modes controlled by `-P` flag:
- **Without `-P`**: Color applies to entire line (prefix + message)
- **With `-P`**: Color only prefix, calculate auto-indent for wrapping

See `output_text()` lines 537-555 for implementation.

### Error Handling
- Error messages use stderr: `output_error "message" >&2`
- Internal logging (for trap handlers) uses full timestamps and no color for reliability
- Level-based return codes: error level returns 1, others return 0

## Version Management

Update three locations when changing version:
1. Header comment (line 7)
2. Changelog (lines 14-20)
3. `output_library_info()` function (line 1046)

Commit format: Follow conventional commits
- `feat:` for new functions
- `fix:` for bug fixes
- `docs:` for documentation updates
- `refactor:` for code improvements without behavior changes

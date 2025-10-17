# Text Handler Library

A comprehensive Bash output library for text formatting, colors, and display.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Bash](https://img.shields.io/badge/Bash-4.0%2B-green.svg)](https://www.gnu.org/software/bash/)
[![CI](https://github.com/tkirkland/string_output.sh/workflows/CI/badge.svg)](https://github.com/tkirkland/string_output.sh/actions/workflows/ci.yml)
[![Release](https://github.com/tkirkland/string_output.sh/workflows/Release/badge.svg)](https://github.com/tkirkland/string_output.sh/actions/workflows/release.yml)

## Features

- 🎨 **ANSI Colors & Styles** - Full color support with bold, dim, and underline
- 📏 **Text Formatting** - Word wrapping, truncation, and alignment
- 📊 **Leveled Messaging** - Info, success, warning, error, and internal logging
- 🎁 **Decorative Elements** - Boxes, headers, separators, and tables
- 🔄 **Progress Indicators** - Spinners and progress bars
- 💬 **Interactive Prompts** - Yes/no confirmation dialogs
- 📝 **File Logging** - Optional logging to files without colors
- 🖥️ **Terminal Detection** - Auto-detect color support and terminal width

## Installation

Simply source the library in your Bash script:

```bash
source ./string_output.sh
```

Or download directly:

```bash
curl -O https://raw.githubusercontent.com/tkirkland/string_output.sh/master/string_output.sh
```

## Quick Start

```bash
#!/bin/bash
source ./string_output.sh

# Simple messages
output_info "Processing configuration..."
output_success "Installation complete!"
output_warning "Disk space running low"
output_error "Failed to connect to server"

# Wrapped output with colored prefix
output_text -P -w -l warning "This is a very long message that will automatically wrap with continuation lines indented properly under the message content"

# Tables
output_table \
  "Name|Age|City" \
  "Alice|30|New York" \
  "Bob|25|Los Angeles"

# Progress indicators
for i in {1..100}; do
  output_progress "$i" 100 "Installing"
  sleep 0.05
done

# Confirmation prompts
if output_confirm "Continue with installation?" "y"; then
  output_success "User confirmed"
fi
```

## Core Functions

### Message Levels

- `output_info` - Blue [INFO] messages
- `output_success` - Green [SUCCESS] messages
- `output_warning` - Yellow [WARNING] messages
- `output_error` - Red [ERROR] messages (to stderr)
- `output_internal` - Timestamped logging for trap handlers

### Decorative Output

- `output_box` - Draw Unicode boxes around text
- `output_header` - Section headers with decorative boxes
- `output_separator` - Horizontal separator lines
- `output_table` - Formatted tables with auto-sized columns

### Progress & Interaction

- `output_spinner` - Animated spinner for background processes
- `output_progress` - Progress bar with percentage
- `output_confirm` - Interactive yes/no prompts

### Text Manipulation

- `wrap_text` - Word wrap with intelligent indentation
- `truncate_text` - Truncate with "..." indicator
- `align_text` - Left, center, or right alignment

## Advanced Usage

### Main Output Function

The `output_text` function provides comprehensive control:

```bash
output_text [OPTIONS] <message>

Options:
  -c, --color <name>        Color: red|green|yellow|blue|magenta|cyan|white
  -s, --style <name>        Style: bold|dim|underline
  -l, --level <name>        Level: info|success|warning|error|internal
  -n, --no-newline          Suppress trailing newline
  -t, --timestamp           Add timestamp prefix
  -f, --file <path>         Also log to file (without colors)
  -w, --wrap                Enable word wrapping
  -W, --width <num>         Set maximum width (default: 79)
  -T, --truncate            Truncate long lines
  -a, --align <type>        Alignment: left|center|right
  -i, --indent <num>        Indentation width in spaces
  -p, --prefix <text>       Custom prefix text
  -P, --prefix-color-only   Color only prefix, auto-indent continuation
```

### Examples

```bash
# Custom colored output
output_text -c magenta -s bold "Important notice"

# Wrapped text with custom width
output_text -w -W 60 "Long text that needs to wrap at 60 columns"

# File logging
output_text -l info -f /var/log/script.log "Logged to both terminal and file"

# Timestamp for debugging
output_text -t -l internal "Cleanup handler triggered"

# Centered text
output_text -a center -W 80 "Centered Title"
```

## Configuration

Control behavior with global variables:

```bash
th_use_color=0      # Disable colors (auto-detected by default)
th_verbosity=0      # Quiet mode (suppress non-error output)
th_term_width=100   # Override terminal width detection
```

## Requirements

- **Bash**: 4.0 or later
- **Optional**: `tput` for terminal capability detection
- **Optional**: `shellcheck` for linting

## Development

### Testing

```bash
# Source and test
source ./string_output.sh
output_library_info

# Run linting
shellcheck string_output.sh
```

### Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Follow the existing code style and documentation format
4. Test your changes thoroughly
5. Commit with conventional commit messages (`feat:`, `fix:`, `docs:`, etc.)
6. Push to your branch and open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Changelog

### v1.1.0 (2025-10-16)
- Enhanced `-P -w` combination for professional wrapped output
- Continuation lines now auto-indent under message content
- First line skips indent when prefix is present

### v1.0.1
- Initial documented release

## Author

**tkirkland**

## Acknowledgments

- Inspired by modern CLI tools and their output formatting
- Unicode box-drawing characters for visual appeal
- ANSI escape sequences for color support

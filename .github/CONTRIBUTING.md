# Contributing to Text Handler Library

Thank you for considering contributing to the Text Handler Library! This document provides guidelines for contributing to the project.

## Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help maintain a welcoming environment

## How to Contribute

### Reporting Bugs

1. Check if the bug has already been reported in [Issues](https://github.com/tkirkland/string_output.sh/issues)
2. If not, create a new issue with:
   - Clear title and description
   - Steps to reproduce
   - Expected vs actual behavior
   - Bash version and OS details
   - Code example demonstrating the issue

### Suggesting Features

1. Check existing issues and discussions
2. Create a new issue with:
   - Clear use case description
   - Proposed API/interface
   - Example usage
   - Why this benefits the library

### Pull Requests

1. **Fork and Clone**
   ```bash
   git clone https://github.com/YOUR-USERNAME/string_output.sh.git
   cd string_output.sh
   ```

2. **Create a Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make Changes**
   - Follow the existing code style
   - Add Google-style docstrings for new functions
   - Update the changelog in the library header
   - Test your changes thoroughly

4. **Test Your Changes**
   ```bash
   # Syntax check
   bash -n string_output.sh

   # ShellCheck
   shellcheck string_output.sh

   # Manual testing
   source ./string_output.sh
   # Test your new functionality
   ```

5. **Commit Changes**
   Use conventional commit messages:
   ```bash
   git commit -m "feat: add new function for X"
   git commit -m "fix: correct behavior in Y"
   git commit -m "docs: update README for Z"
   ```

   Commit types:
   - `feat:` - New feature
   - `fix:` - Bug fix
   - `docs:` - Documentation only
   - `style:` - Formatting (no code change)
   - `refactor:` - Code restructuring
   - `test:` - Adding tests
   - `chore:` - Maintenance tasks

6. **Push and Create PR**
   ```bash
   git push origin feature/your-feature-name
   ```
   Then create a Pull Request on GitHub

## Code Style Guidelines

### Function Documentation

All functions must include Google-style docstrings:

```bash
#######################################
# Brief description
# Detailed description of what the function does.
# Globals:
#   VARIABLE - Description
# Arguments:
#   $1 - Description (default: value)
#   $2 - Description
# Outputs:
#   Description of output
# Returns:
#   0 - Success
#   1 - Error condition
# Example:
#   function_name "example"
#######################################
function_name() {
  # Implementation
}
```

### Naming Conventions

- **Constants**: `UPPERCASE` with `TH_` prefix
  ```bash
  readonly TH_COLOR_RED='\033[0;31m'
  ```

- **Public Functions**: `snake_case` with `output_` prefix
  ```bash
  output_new_feature() { ... }
  ```

- **Private Functions**: Leading underscore
  ```bash
  _internal_helper() { ... }
  ```

- **Local Variables**: `snake_case`
  ```bash
  local my_variable="value"
  ```

### Code Organization

1. Use `local` for all function-scoped variables
2. Use `readonly` for constants
3. Declare arrays with `-a` flag: `local -a array_name`
4. Use double brackets for conditionals: `[[ ... ]]`
5. Quote variables: `"$variable"`
6. Use command substitution with `$()` not backticks

### ANSI Code Handling

When working with text that may contain ANSI codes:

1. Strip codes for length calculations: `strip_ansi "$text"`
2. Preserve codes in output
3. Test with both colored and non-colored input

## Version Updates

When updating the version:

1. Update the header comment (line ~7)
2. Add changelog entry (lines ~14-20)
3. Update `output_library_info()` function
4. Follow semantic versioning:
   - MAJOR: Breaking changes
   - MINOR: New features (backward compatible)
   - PATCH: Bug fixes

## Testing Checklist

Before submitting a PR:

- [ ] Code passes `bash -n string_output.sh`
- [ ] Code passes `shellcheck string_output.sh`
- [ ] Library sources without errors
- [ ] New functions have complete docstrings
- [ ] Examples in docstrings are accurate
- [ ] Existing functions still work (regression test)
- [ ] Edge cases tested (empty input, long strings, ANSI codes)
- [ ] Works with `th_use_color=0` (no color mode)
- [ ] Documentation updated if needed
- [ ] Changelog updated

## Questions?

Feel free to:
- Open an issue for discussion
- Ask in your Pull Request
- Check existing documentation in `CLAUDE.md` and `README.md`

Thank you for contributing! 🎉

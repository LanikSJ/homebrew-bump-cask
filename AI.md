# AI Rules & Project Standards for homebrew-bump-cask

## Repository Overview

homebrew-bump-cask is a GitHub Action for automatically updating Homebrew casks.

## Code Standards and Practices

### Action Standards

- Follow GitHub Actions best practices.
- Implement proper input validation and error handling.
- Include comprehensive logging and debugging options.
- Provide clear error messages for common failure scenarios.

### Ruby Code Standards

- Write clear, maintainable Ruby code.
- Include proper error handling and validation.
- Add tests for new functionality.
- Follow Ruby style guidelines.

### Documentation Standards

- Include clear usage examples and workflow templates.
- Document all inputs, outputs, and required permissions.
- Provide troubleshooting guidance for common issues.
- Use markdown formatting consistently.

### Markdown Compliance Requirements (MANDATORY)

- **ALL markdown files (.md) MUST pass markdownlint validation with zero errors or warnings**
- Run `markdownlint <filename>` on every markdown file before considering it complete
- Follow the project's `.markdownlint.json` configuration strictly
- Address ALL markdownlint issues immediately - no exceptions or workarounds
- Common requirements include:
  - Maximum line length of 80 characters (MD013)
  - Consistent heading styles and hierarchy
  - Proper list formatting and indentation
  - Blank lines around headings and code blocks
  - Consistent link and reference formatting
  - No trailing whitespace
  - Files must end with newlines
  - Proper table formatting when applicable
- Use `markdownlint --fix <filename>` for auto-fixable issues when available
- Validate markdown files in CI/CD pipelines where applicable

## Development Guidelines

### When Making Changes

- Preserve existing functionality unless explicitly asked to change it
- Update documentation when modifying action behavior or adding features
- Test changes across different Homebrew cask scenarios
- **Always run markdownlint and fix all issues in markdown files before considering changes complete**

### Homebrew Integration Standards

- Handle various cask formats and update patterns.
- Implement proper version checking and comparison logic.
- Provide clear feedback for cask update operations.
- Test against real Homebrew casks and repositories.

## GitHub & Automation Standards

These rules apply specifically to files in `.github/*` (workflows, templates, and documentation).

### Quality Gates (MANDATORY)

Before completing any change in `.github/`:

1. ✅ Run `markdownlint` validation (if .md file).
2. ✅ Ensure project standards are followed.
3. ✅ Verify contribution guidelines are up-to-date.
4. ✅ Check that automation maintains project standards.

### Templates and Workflows

- Ensure issue and pull request templates provide clear, actionable guidelines.
- Include project-specific troubleshooting sections in templates.
- Reference existing project documentation and standards.

### Documentation standards in .github/

- `.github/CONTRIBUTING.md` must include:
  - Development environment setup instructions.
  - Testing requirements and procedures.
  - Documentation standards for new features.
  - Project-specific contribution guidelines.

### Automation and CI/CD

- Project workflows must include automated testing stages.
- Code quality checks must be integrated into CI/CD.
- Release automation must be properly configured.

### Error Prevention

- NEVER generate markdown that violates line length or formatting rules.
- ALWAYS cross-reference with existing project practices before making changes.
- ENSURE all links and references are valid and current.
- VALIDATE that new requirements don't conflict with established workflows.

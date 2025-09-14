# Development Setup

This guide will help you set up a development environment for contributing to the framework documentation.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Python 3.8 or higher**
- **Git** for version control
- **pip** package manager
- **Text editor or IDE** (VS Code, PyCharm, etc.)

## Getting Started

### 1. Fork and Clone the Repository

1. Fork the repository on GitHub
2. Clone your fork locally:

```bash
git clone https://github.com/yourusername/framework-docs.git
cd framework-docs
```

### 2. Set Up Virtual Environment

Create and activate a virtual environment:

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies

Install the required dependencies:

```bash
pip install -r requirements.txt
```

### 4. Verify Installation

Test that MkDocs is working correctly:

```bash
mkdocs --version
```

You should see output similar to:
```
mkdocs, version 1.5.0 from /path/to/mkdocs
```

## Development Workflow

### Running the Development Server

Start the local development server:

```bash
mkdocs serve
```

This will:
- Start a local server at `http://localhost:8000`
- Watch for file changes and auto-reload
- Show build warnings and errors

### Making Changes

1. **Edit Documentation**: Modify files in the `docs/` directory
2. **Update Navigation**: Edit `mkdocs.yml` if adding new pages
3. **Preview Changes**: Check your changes at `http://localhost:8000`
4. **Test Build**: Run `mkdocs build` to ensure no errors

### Building for Production

Generate the static site:

```bash
mkdocs build
```

The built site will be in the `site/` directory.

## Project Structure

```
framework-docs/
├── docs/                    # Documentation source files
│   ├── index.md            # Homepage
│   ├── getting-started/    # Getting started guides
│   ├── user-guide/         # User guides
│   ├── api-reference/      # API documentation
│   ├── examples/           # Code examples
│   ├── contributing/       # Contribution guidelines
│   └── changelog.md        # Version history
├── .github/
│   └── workflows/          # GitHub Actions workflows
├── mkdocs.yml              # MkDocs configuration
├── requirements.txt        # Python dependencies
├── netlify.toml           # Netlify configuration
└── README.md              # Project README
```

## Writing Documentation

### Markdown Guidelines

- Use clear, concise language
- Include code examples where appropriate
- Use proper heading hierarchy (H1, H2, H3, etc.)
- Add links to related sections

### Code Examples

Format code blocks with syntax highlighting:

```python
from framework import Framework

app = Framework()
app.initialize()
```

### Admonitions

Use admonitions for important information:

```markdown
!!! note
    This is a note admonition.

!!! warning
    This is a warning admonition.

!!! tip
    This is a tip admonition.
```

### Internal Links

Link to other documentation pages:

```markdown
See the [Installation Guide](../getting-started/installation.md) for details.
```

## Testing Your Changes

### Local Testing

1. **Start development server**: `mkdocs serve`
2. **Check all pages load correctly**
3. **Verify navigation works**
4. **Test internal links**
5. **Check code examples**

### Build Testing

1. **Clean build**: `mkdocs build --clean`
2. **Check for warnings or errors**
3. **Verify all files are generated**

### Link Checking

Use a link checker to verify all links work:

```bash
# Install link checker
pip install linkchecker

# Check links (after building)
linkchecker site/
```

## Submitting Changes

### Before Submitting

1. **Test thoroughly**: Ensure all changes work correctly
2. **Check formatting**: Verify markdown formatting is correct
3. **Update navigation**: Add new pages to `mkdocs.yml` if needed
4. **Write clear commit messages**

### Pull Request Process

1. **Create a branch**: `git checkout -b feature/your-feature-name`
2. **Make your changes**
3. **Commit changes**: `git commit -m "Add feature description"`
4. **Push to your fork**: `git push origin feature/your-feature-name`
5. **Create pull request** on GitHub

### Pull Request Guidelines

- **Clear title**: Describe what the PR does
- **Detailed description**: Explain the changes and why they're needed
- **Link issues**: Reference any related issues
- **Screenshots**: Include screenshots for UI changes

## Common Issues

### MkDocs Build Errors

**Issue**: `Config value: 'theme': Unrecognised theme name`
**Solution**: Ensure `mkdocs-material` is installed

**Issue**: `WARNING - Documentation file 'path/file.md' not found`
**Solution**: Check file paths in `mkdocs.yml` navigation

### Development Server Issues

**Issue**: Server not auto-reloading
**Solution**: Restart the server with `mkdocs serve`

**Issue**: Port already in use
**Solution**: Use a different port: `mkdocs serve -a localhost:8001`

## Tools and Extensions

### Recommended VS Code Extensions

- **Markdown All in One**: Enhanced markdown support
- **markdownlint**: Markdown linting
- **Python**: Python language support
- **YAML**: YAML syntax highlighting

### Useful Commands

```bash
# Serve with specific address/port
mkdocs serve -a localhost:8001

# Build with verbose output
mkdocs build --verbose

# Clean build directory
mkdocs build --clean

# Check configuration
mkdocs config
```

## Getting Help

If you need help with development setup:

1. **Check existing issues** on GitHub
2. **Read the MkDocs documentation**: https://www.mkdocs.org/
3. **Ask questions** in GitHub Discussions
4. **Contact maintainers** via GitHub issues

## Next Steps

- Review [Contributing Guidelines](guidelines.md)
- Check out [Basic Examples](../examples/basic-examples.md)
- Read about [Best Practices](../user-guide/best-practices.md)

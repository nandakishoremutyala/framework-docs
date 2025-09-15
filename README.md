# Framework Documentation

A comprehensive documentation site for Framework built with MkDocs Material.

## Features

- 📚 **Comprehensive Documentation** - Complete guides from installation to advanced usage
- 🎨 **Beautiful UI** - Built with Material Design using MkDocs Material theme
- 🔍 **Full-text Search** - Instant search across all documentation
- 📱 **Responsive Design** - Works perfectly on desktop and mobile devices
- 🌙 **Dark/Light Mode** - Toggle between themes
- 🚀 **Fast Loading** - Optimized static site generation
- 📖 **Easy Navigation** - Organized sections with clear hierarchy

## Quick Start

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Installation

1. Clone this repository:
   ```bash
   git clone <your-repo-url>
   cd framework-docs
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Serve locally:
   ```bash
   mkdocs serve
   ```

4. Open your browser to `http://localhost:8000`

## Building for Production

Generate static files for deployment:

```bash
mkdocs build
```

The built site will be in the `site/` directory.

## Deployment

This site is automatically deployed to GitHub Pages using GitHub Actions. Every push to the `main` branch triggers a new deployment.

### GitHub Pages Configuration

- **Source**: GitHub Actions
- **Build command**: `mkdocs build`
- **Publish directory**: `site`
- **Live URL**: https://nandakishoremutyala.github.io/framework-docs

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
├── mkdocs.yml              # MkDocs configuration
├── requirements.txt        # Python dependencies
└── README.md              # This file
```

## Customization

### Theme Configuration

The site uses MkDocs Material theme with extensive customization in `mkdocs.yml`:

- **Colors**: Indigo primary with dark/light mode support
- **Navigation**: Tabs, sections, and instant loading
- **Features**: Code highlighting, search, social links
- **Extensions**: Admonitions, code blocks, diagrams

### Adding Content

1. Create new markdown files in the `docs/` directory
2. Update the navigation in `mkdocs.yml`
3. Use Material Design components and extensions

### Styling

Customize the appearance by:

- Modifying theme settings in `mkdocs.yml`
- Adding custom CSS in `docs/stylesheets/`
- Using Material Design color schemes

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally with `mkdocs serve`
5. Submit a pull request

## License

This documentation is licensed under the MIT License. See LICENSE file for details.

## Support

- **Documentation**: [Live Site](https://nandakishoremutyala.github.io/framework-docs)
- **Issues**: [GitHub Issues](https://github.com/yourusername/framework-docs/issues)
- **Framework**: [Main Repository](https://github.com/yourusername/framework)

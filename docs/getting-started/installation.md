# Installation

This guide will walk you through installing Framework on your system.

## System Requirements

Before installing Framework, ensure your system meets the following requirements:

- **Python**: 3.8 or higher
- **Operating System**: Windows, macOS, or Linux
- **Memory**: At least 512MB RAM available
- **Disk Space**: 100MB free space

## Installation Methods

### Method 1: Using pip (Recommended)

The easiest way to install Framework is using pip:

```bash
pip install framework
```

For the latest development version:

```bash
pip install git+https://github.com/yourusername/framework.git
```

### Method 2: Using conda

If you prefer conda:

```bash
conda install -c conda-forge framework
```

### Method 3: From Source

For developers who want to contribute or need the latest features:

```bash
# Clone the repository
git clone https://github.com/yourusername/framework.git
cd framework

# Install in development mode
pip install -e .
```

## Verify Installation

After installation, verify that Framework is installed correctly:

```bash
framework --version
```

You should see output similar to:

```
Framework version 1.0.0
```

## Virtual Environment (Recommended)

It's recommended to install Framework in a virtual environment:

```bash
# Create virtual environment
python -m venv framework-env

# Activate virtual environment
# On Windows:
framework-env\Scripts\activate
# On macOS/Linux:
source framework-env/bin/activate

# Install Framework
pip install framework
```

## Troubleshooting

### Common Issues

#### Permission Errors
If you encounter permission errors on macOS/Linux:

```bash
pip install --user framework
```

#### Python Version Issues
Ensure you're using Python 3.8+:

```bash
python --version
```

#### Network Issues
If you're behind a corporate firewall:

```bash
pip install --trusted-host pypi.org --trusted-host pypi.python.org framework
```

### Getting Help

If you encounter issues during installation:

1. Check the [GitHub Issues](https://github.com/yourusername/framework/issues)
2. Join our [Discord community](https://discord.gg/framework)
3. Email support at support@framework.dev

## Next Steps

Once Framework is installed, proceed to the [Quick Start Guide](quick-start.md) to create your first project.

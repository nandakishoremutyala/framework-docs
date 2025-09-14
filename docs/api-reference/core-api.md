# Core API Reference

This document provides comprehensive reference documentation for the framework's core API.

## Overview

The Core API provides the fundamental functionality and building blocks for framework applications.

## Classes

### Framework

The main framework class that provides core functionality.

#### Constructor

```python
Framework(config=None, debug=False)
```

**Parameters:**
- `config` (dict, optional): Configuration dictionary
- `debug` (bool, optional): Enable debug mode

**Example:**
```python
from framework import Framework

app = Framework(config={'timeout': 30}, debug=True)
```

#### Methods

##### `initialize()`

Initialize the framework with the provided configuration.

**Returns:** `bool` - Success status

**Example:**
```python
success = app.initialize()
if success:
    print("Framework initialized successfully")
```

##### `run()`

Start the framework application.

**Returns:** `None`

**Example:**
```python
app.run()
```

##### `shutdown()`

Gracefully shutdown the framework.

**Returns:** `None`

**Example:**
```python
app.shutdown()
```

### ConfigManager

Handles configuration management for the framework.

#### Constructor

```python
ConfigManager(config_path=None)
```

**Parameters:**
- `config_path` (str, optional): Path to configuration file

#### Methods

##### `load_config(path)`

Load configuration from file.

**Parameters:**
- `path` (str): Path to configuration file

**Returns:** `dict` - Configuration dictionary

##### `get(key, default=None)`

Get configuration value by key.

**Parameters:**
- `key` (str): Configuration key
- `default` (any, optional): Default value if key not found

**Returns:** `any` - Configuration value

##### `set(key, value)`

Set configuration value.

**Parameters:**
- `key` (str): Configuration key
- `value` (any): Configuration value

**Returns:** `None`

## Functions

### `create_app(config=None)`

Factory function to create a framework application.

**Parameters:**
- `config` (dict, optional): Configuration dictionary

**Returns:** `Framework` - Framework instance

**Example:**
```python
from framework import create_app

app = create_app({'debug': True})
```

### `get_version()`

Get the current framework version.

**Returns:** `str` - Version string

**Example:**
```python
from framework import get_version

version = get_version()
print(f"Framework version: {version}")
```

## Constants

### `DEFAULT_CONFIG`

Default configuration dictionary.

```python
DEFAULT_CONFIG = {
    'debug': False,
    'timeout': 30,
    'max_retries': 3
}
```

### `VERSION`

Current framework version string.

## Exceptions

### `FrameworkError`

Base exception class for framework-related errors.

### `ConfigurationError`

Raised when there are configuration-related issues.

### `InitializationError`

Raised when framework initialization fails.

## Usage Examples

### Basic Usage

```python
from framework import Framework

# Create and initialize framework
app = Framework({'debug': True})
app.initialize()

# Run the application
app.run()
```

### Advanced Configuration

```python
from framework import Framework, ConfigManager

# Load configuration from file
config_manager = ConfigManager('config.json')
config = config_manager.load_config()

# Create framework with custom config
app = Framework(config)
app.initialize()
app.run()
```

## See Also

- [Utilities API](utilities.md)
- [Extensions API](extensions.md)
- [Getting Started](../getting-started/installation.md)

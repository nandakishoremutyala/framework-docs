# Utilities API Reference

This document provides reference documentation for the framework's utility functions and classes.

## Overview

The Utilities API provides helper functions and classes that support common operations and tasks within the framework.

## String Utilities

### `format_string(template, **kwargs)`

Format a string template with provided keyword arguments.

**Parameters:**
- `template` (str): String template with placeholders
- `**kwargs`: Keyword arguments for formatting

**Returns:** `str` - Formatted string

**Example:**
```python
from framework.utils import format_string

result = format_string("Hello {name}!", name="World")
# Returns: "Hello World!"
```

### `sanitize_input(input_string)`

Sanitize user input by removing potentially harmful characters.

**Parameters:**
- `input_string` (str): Input string to sanitize

**Returns:** `str` - Sanitized string

## File Utilities

### `read_file(file_path, encoding='utf-8')`

Read content from a file.

**Parameters:**
- `file_path` (str): Path to the file
- `encoding` (str, optional): File encoding

**Returns:** `str` - File content

**Example:**
```python
from framework.utils import read_file

content = read_file('config.txt')
```

### `write_file(file_path, content, encoding='utf-8')`

Write content to a file.

**Parameters:**
- `file_path` (str): Path to the file
- `content` (str): Content to write
- `encoding` (str, optional): File encoding

**Returns:** `bool` - Success status

### `ensure_directory(directory_path)`

Ensure a directory exists, creating it if necessary.

**Parameters:**
- `directory_path` (str): Path to the directory

**Returns:** `bool` - Success status

## Data Utilities

### `deep_merge(dict1, dict2)`

Recursively merge two dictionaries.

**Parameters:**
- `dict1` (dict): First dictionary
- `dict2` (dict): Second dictionary

**Returns:** `dict` - Merged dictionary

**Example:**
```python
from framework.utils import deep_merge

config1 = {'database': {'host': 'localhost'}}
config2 = {'database': {'port': 5432}}
merged = deep_merge(config1, config2)
# Returns: {'database': {'host': 'localhost', 'port': 5432}}
```

### `flatten_dict(nested_dict, separator='.')`

Flatten a nested dictionary.

**Parameters:**
- `nested_dict` (dict): Nested dictionary to flatten
- `separator` (str, optional): Key separator

**Returns:** `dict` - Flattened dictionary

## Validation Utilities

### `validate_email(email)`

Validate an email address format.

**Parameters:**
- `email` (str): Email address to validate

**Returns:** `bool` - True if valid, False otherwise

### `validate_url(url)`

Validate a URL format.

**Parameters:**
- `url` (str): URL to validate

**Returns:** `bool` - True if valid, False otherwise

### `validate_json(json_string)`

Validate JSON string format.

**Parameters:**
- `json_string` (str): JSON string to validate

**Returns:** `bool` - True if valid, False otherwise

## Logging Utilities

### `setup_logger(name, level='INFO', format_string=None)`

Set up a logger with specified configuration.

**Parameters:**
- `name` (str): Logger name
- `level` (str, optional): Logging level
- `format_string` (str, optional): Custom format string

**Returns:** `Logger` - Configured logger instance

**Example:**
```python
from framework.utils import setup_logger

logger = setup_logger('my_app', level='DEBUG')
logger.info('Application started')
```

## Time Utilities

### `get_timestamp(format='%Y-%m-%d %H:%M:%S')`

Get current timestamp as formatted string.

**Parameters:**
- `format` (str, optional): Timestamp format

**Returns:** `str` - Formatted timestamp

### `parse_duration(duration_string)`

Parse duration string into seconds.

**Parameters:**
- `duration_string` (str): Duration string (e.g., '1h30m', '45s')

**Returns:** `int` - Duration in seconds

## Encryption Utilities

### `hash_password(password, salt=None)`

Hash a password using secure hashing algorithm.

**Parameters:**
- `password` (str): Password to hash
- `salt` (str, optional): Salt for hashing

**Returns:** `str` - Hashed password

### `verify_password(password, hashed_password)`

Verify a password against its hash.

**Parameters:**
- `password` (str): Plain text password
- `hashed_password` (str): Hashed password

**Returns:** `bool` - True if password matches

## Network Utilities

### `is_port_open(host, port, timeout=3)`

Check if a network port is open.

**Parameters:**
- `host` (str): Host address
- `port` (int): Port number
- `timeout` (int, optional): Connection timeout

**Returns:** `bool` - True if port is open

### `get_local_ip()`

Get the local IP address.

**Returns:** `str` - Local IP address

## Usage Examples

### File Operations
```python
from framework.utils import read_file, write_file, ensure_directory

# Ensure directory exists
ensure_directory('/path/to/logs')

# Read configuration
config = read_file('/path/to/config.json')

# Write log file
write_file('/path/to/logs/app.log', 'Application started')
```

### Data Processing
```python
from framework.utils import deep_merge, flatten_dict

# Merge configurations
base_config = {'app': {'name': 'MyApp'}}
user_config = {'app': {'version': '1.0'}}
final_config = deep_merge(base_config, user_config)

# Flatten for environment variables
flat_config = flatten_dict(final_config, separator='_')
```

## See Also

- [Core API](core-api.md)
- [Extensions API](extensions.md)
- [Best Practices](../user-guide/best-practices.md)

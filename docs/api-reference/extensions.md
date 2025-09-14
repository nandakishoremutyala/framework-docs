# Extensions API Reference

This document provides reference documentation for the framework's extension system and available extensions.

## Overview

The Extensions API allows you to extend the framework's functionality through a plugin-based architecture. Extensions can add new features, modify existing behavior, or integrate with external systems.

## Extension Base Class

### `Extension`

Base class for all framework extensions.

#### Constructor

```python
Extension(name, version='1.0.0')
```

**Parameters:**
- `name` (str): Extension name
- `version` (str, optional): Extension version

#### Methods

##### `initialize(framework_instance)`

Initialize the extension with the framework instance.

**Parameters:**
- `framework_instance` (Framework): Framework instance

**Returns:** `bool` - Success status

##### `activate()`

Activate the extension.

**Returns:** `bool` - Success status

##### `deactivate()`

Deactivate the extension.

**Returns:** `bool` - Success status

##### `get_info()`

Get extension information.

**Returns:** `dict` - Extension information

## Extension Manager

### `ExtensionManager`

Manages framework extensions.

#### Constructor

```python
ExtensionManager(framework_instance)
```

**Parameters:**
- `framework_instance` (Framework): Framework instance

#### Methods

##### `load_extension(extension_path)`

Load an extension from file path.

**Parameters:**
- `extension_path` (str): Path to extension file

**Returns:** `Extension` - Loaded extension instance

##### `register_extension(extension)`

Register an extension instance.

**Parameters:**
- `extension` (Extension): Extension instance

**Returns:** `bool` - Success status

##### `activate_extension(name)`

Activate an extension by name.

**Parameters:**
- `name` (str): Extension name

**Returns:** `bool` - Success status

##### `deactivate_extension(name)`

Deactivate an extension by name.

**Parameters:**
- `name` (str): Extension name

**Returns:** `bool` - Success status

##### `list_extensions()`

List all registered extensions.

**Returns:** `list` - List of extension names

## Built-in Extensions

### Database Extension

Provides database connectivity and ORM functionality.

#### Configuration

```python
database_config = {
    'host': 'localhost',
    'port': 5432,
    'database': 'myapp',
    'username': 'user',
    'password': 'password'
}
```

#### Usage

```python
from framework.extensions import DatabaseExtension

db_ext = DatabaseExtension('database', config=database_config)
db_ext.initialize(framework_instance)
db_ext.activate()
```

### Cache Extension

Provides caching functionality with multiple backends.

#### Configuration

```python
cache_config = {
    'backend': 'redis',
    'host': 'localhost',
    'port': 6379,
    'ttl': 3600
}
```

#### Usage

```python
from framework.extensions import CacheExtension

cache_ext = CacheExtension('cache', config=cache_config)
cache_ext.initialize(framework_instance)
cache_ext.activate()
```

### Authentication Extension

Provides user authentication and authorization.

#### Configuration

```python
auth_config = {
    'secret_key': 'your-secret-key',
    'token_expiry': 3600,
    'hash_algorithm': 'bcrypt'
}
```

#### Usage

```python
from framework.extensions import AuthExtension

auth_ext = AuthExtension('auth', config=auth_config)
auth_ext.initialize(framework_instance)
auth_ext.activate()
```

## Creating Custom Extensions

### Basic Extension Example

```python
from framework.extensions import Extension

class MyCustomExtension(Extension):
    def __init__(self):
        super().__init__('my_custom_extension', '1.0.0')
        self.framework = None
    
    def initialize(self, framework_instance):
        self.framework = framework_instance
        # Perform initialization logic
        return True
    
    def activate(self):
        # Activate extension functionality
        self.framework.register_handler('custom_event', self.handle_event)
        return True
    
    def deactivate(self):
        # Deactivate extension functionality
        self.framework.unregister_handler('custom_event', self.handle_event)
        return True
    
    def handle_event(self, event_data):
        # Handle custom events
        print(f"Handling event: {event_data}")
```

### Advanced Extension Example

```python
from framework.extensions import Extension
from framework.utils import setup_logger

class AdvancedExtension(Extension):
    def __init__(self, config=None):
        super().__init__('advanced_extension', '2.0.0')
        self.config = config or {}
        self.logger = setup_logger(f'{self.name}_logger')
        self.active = False
    
    def initialize(self, framework_instance):
        self.framework = framework_instance
        self.logger.info(f"Initializing {self.name}")
        
        # Validate configuration
        if not self._validate_config():
            self.logger.error("Invalid configuration")
            return False
        
        # Set up extension resources
        self._setup_resources()
        return True
    
    def activate(self):
        if self.active:
            return True
        
        self.logger.info(f"Activating {self.name}")
        # Activation logic here
        self.active = True
        return True
    
    def deactivate(self):
        if not self.active:
            return True
        
        self.logger.info(f"Deactivating {self.name}")
        # Cleanup logic here
        self.active = False
        return True
    
    def _validate_config(self):
        required_keys = ['api_key', 'endpoint']
        return all(key in self.config for key in required_keys)
    
    def _setup_resources(self):
        # Set up extension-specific resources
        pass
```

## Extension Lifecycle

1. **Load**: Extension is loaded from file or registered
2. **Initialize**: Extension is initialized with framework instance
3. **Activate**: Extension functionality is activated
4. **Runtime**: Extension operates normally
5. **Deactivate**: Extension functionality is deactivated
6. **Cleanup**: Extension resources are cleaned up

## Best Practices

### Extension Development
- Inherit from the base `Extension` class
- Implement all required methods
- Handle errors gracefully
- Use proper logging
- Clean up resources in deactivate()

### Configuration Management
- Validate configuration in initialize()
- Provide sensible defaults
- Document configuration options
- Support environment variables

### Error Handling
- Return appropriate status codes
- Log errors with context
- Fail gracefully
- Provide meaningful error messages

## Usage Examples

### Loading and Using Extensions

```python
from framework import Framework
from framework.extensions import ExtensionManager

# Create framework instance
app = Framework()
app.initialize()

# Create extension manager
ext_manager = ExtensionManager(app)

# Load and activate extensions
db_ext = ext_manager.load_extension('path/to/database_extension.py')
ext_manager.activate_extension('database')

cache_ext = ext_manager.load_extension('path/to/cache_extension.py')
ext_manager.activate_extension('cache')

# List active extensions
active_extensions = ext_manager.list_extensions()
print(f"Active extensions: {active_extensions}")
```

### Custom Extension Registration

```python
from my_extensions import MyCustomExtension

# Create and register custom extension
custom_ext = MyCustomExtension()
ext_manager.register_extension(custom_ext)
ext_manager.activate_extension('my_custom_extension')
```

## See Also

- [Core API](core-api.md)
- [Utilities API](utilities.md)
- [Advanced Features](../user-guide/advanced-features.md)

# Basic Examples

This section provides basic examples to help you get started with the framework quickly.

## Hello World Example

The simplest possible framework application:

```python
from framework import Framework

# Create framework instance
app = Framework()

# Initialize the framework
app.initialize()

# Add a simple handler
@app.route('/')
def hello_world():
    return "Hello, World!"

# Run the application
if __name__ == '__main__':
    app.run()
```

## Configuration Example

Using configuration with the framework:

```python
from framework import Framework

# Configuration dictionary
config = {
    'debug': True,
    'host': '0.0.0.0',
    'port': 8080,
    'database': {
        'host': 'localhost',
        'port': 5432,
        'name': 'myapp'
    }
}

# Create framework with configuration
app = Framework(config=config)
app.initialize()

# Access configuration
db_host = app.config.get('database.host')
print(f"Database host: {db_host}")

app.run()
```

## Request Handling Example

Handling different types of requests:

```python
from framework import Framework

app = Framework()
app.initialize()

@app.route('/', methods=['GET'])
def index():
    return "Welcome to the Framework!"

@app.route('/users', methods=['GET'])
def get_users():
    return {"users": ["Alice", "Bob", "Charlie"]}

@app.route('/users', methods=['POST'])
def create_user():
    # Handle user creation
    return {"message": "User created successfully"}

@app.route('/users/<user_id>', methods=['GET'])
def get_user(user_id):
    return {"user_id": user_id, "name": f"User {user_id}"}

app.run()
```

## Database Integration Example

Using the database extension:

```python
from framework import Framework
from framework.extensions import DatabaseExtension

# Database configuration
db_config = {
    'host': 'localhost',
    'port': 5432,
    'database': 'myapp',
    'username': 'user',
    'password': 'password'
}

# Create and configure framework
app = Framework()
app.initialize()

# Add database extension
db_ext = DatabaseExtension('database', config=db_config)
db_ext.initialize(app)
db_ext.activate()

@app.route('/users')
def get_users():
    # Use database connection
    users = app.db.query('SELECT * FROM users')
    return {"users": users}

app.run()
```

## Middleware Example

Adding middleware to your application:

```python
from framework import Framework

app = Framework()
app.initialize()

# Logging middleware
@app.middleware
def logging_middleware(request, response):
    print(f"Request: {request.method} {request.path}")
    return response

# Authentication middleware
@app.middleware
def auth_middleware(request, response):
    if request.path.startswith('/admin'):
        # Check authentication
        if not request.headers.get('Authorization'):
            return {"error": "Authentication required"}, 401
    return response

@app.route('/')
def index():
    return "Public endpoint"

@app.route('/admin')
def admin():
    return "Admin endpoint"

app.run()
```

## Error Handling Example

Implementing error handling:

```python
from framework import Framework
from framework.exceptions import FrameworkError

app = Framework()
app.initialize()

@app.error_handler(404)
def not_found(error):
    return {"error": "Resource not found"}, 404

@app.error_handler(500)
def internal_error(error):
    return {"error": "Internal server error"}, 500

@app.error_handler(FrameworkError)
def framework_error(error):
    return {"error": str(error)}, 400

@app.route('/error')
def trigger_error():
    raise FrameworkError("Something went wrong!")

app.run()
```

## Async Example

Using asynchronous operations:

```python
import asyncio
from framework import Framework

app = Framework()
app.initialize()

@app.route('/async')
async def async_handler():
    # Simulate async operation
    await asyncio.sleep(1)
    return {"message": "Async operation completed"}

@app.route('/fetch-data')
async def fetch_data():
    # Simulate fetching data from external API
    data = await fetch_external_data()
    return {"data": data}

async def fetch_external_data():
    # Simulate external API call
    await asyncio.sleep(0.5)
    return {"result": "External data"}

app.run()
```

## Testing Example

Basic testing setup:

```python
import unittest
from framework import Framework

class TestFramework(unittest.TestCase):
    def setUp(self):
        self.app = Framework({'testing': True})
        self.app.initialize()
        self.client = self.app.test_client()
    
    def test_index(self):
        response = self.client.get('/')
        self.assertEqual(response.status_code, 200)
    
    def test_api_endpoint(self):
        response = self.client.get('/api/users')
        self.assertEqual(response.status_code, 200)
        self.assertIn('users', response.json)
    
    def test_post_request(self):
        data = {'name': 'John Doe', 'email': 'john@example.com'}
        response = self.client.post('/api/users', json=data)
        self.assertEqual(response.status_code, 201)

if __name__ == '__main__':
    unittest.main()
```

## CLI Application Example

Creating a command-line interface:

```python
from framework import Framework
from framework.cli import CLI

app = Framework()
app.initialize()

cli = CLI(app)

@cli.command('hello')
def hello_command(name='World'):
    """Say hello to someone."""
    print(f"Hello, {name}!")

@cli.command('users')
def list_users():
    """List all users."""
    users = app.db.query('SELECT * FROM users')
    for user in users:
        print(f"- {user['name']} ({user['email']})")

if __name__ == '__main__':
    cli.run()
```

## Next Steps

After trying these basic examples:

1. Explore [Advanced Features](../user-guide/advanced-features.md)
2. Check out [Advanced Examples](advanced-examples.md)
3. Review [Best Practices](../user-guide/best-practices.md)
4. Read the [API Reference](../api-reference/core-api.md)

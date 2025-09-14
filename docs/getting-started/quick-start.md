# Quick Start

This guide will help you create your first Framework application in just a few minutes.

## Create Your First Project

Let's create a simple "Hello World" application:

```bash
# Create a new project
framework init hello-world

# Navigate to the project directory
cd hello-world
```

This creates a new directory with the following structure:

```
hello-world/
├── src/
│   └── main.py
├── config/
│   └── settings.yml
├── tests/
│   └── test_main.py
└── requirements.txt
```

## Project Structure

- **`src/`** - Your application source code
- **`config/`** - Configuration files
- **`tests/`** - Unit tests
- **`requirements.txt`** - Project dependencies

## Your First Application

The generated `src/main.py` contains a simple example:

```python
from framework import App, Route

app = App()

@app.route('/')
def hello_world():
    return "Hello, Framework!"

@app.route('/user/<name>')
def hello_user(name):
    return f"Hello, {name}!"

if __name__ == '__main__':
    app.run()
```

## Running Your Application

Start your application:

```bash
framework serve
```

Your application will be available at `http://localhost:8000`.

## Testing Your Application

Framework includes built-in testing support:

```bash
# Run tests
framework test

# Run tests with coverage
framework test --coverage
```

## Configuration

Customize your application using `config/settings.yml`:

```yaml
app:
  name: "Hello World App"
  debug: true
  port: 8000

database:
  url: "sqlite:///app.db"
  
logging:
  level: "INFO"
  format: "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
```

## Adding Routes

Add more functionality by creating additional routes:

```python
@app.route('/api/status')
def status():
    return {
        "status": "ok",
        "version": "1.0.0",
        "timestamp": datetime.now().isoformat()
    }

@app.route('/api/users', methods=['POST'])
def create_user():
    data = request.get_json()
    # Process user creation
    return {"message": "User created successfully"}
```

## Middleware

Add middleware for common functionality:

```python
from framework.middleware import CORSMiddleware, LoggingMiddleware

app.add_middleware(CORSMiddleware)
app.add_middleware(LoggingMiddleware)
```

## Next Steps

Now that you have a basic application running, explore these topics:

- **[Configuration](configuration.md)** - Learn about advanced configuration options
- **[Basic Usage](../user-guide/basic-usage.md)** - Understand core concepts
- **[API Reference](../api-reference/core-api.md)** - Detailed API documentation
- **[Examples](../examples/basic-examples.md)** - More example applications

## Common Commands

Here are the most commonly used Framework commands:

```bash
# Create new project
framework init <project-name>

# Start development server
framework serve

# Run tests
framework test

# Build for production
framework build

# Deploy application
framework deploy

# Show help
framework --help
```

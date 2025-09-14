# Basic Usage

This guide covers the fundamental concepts and basic usage patterns of Framework.

## Core Concepts

### Applications

The `App` class is the central component of any Framework application:

```python
from framework import App

app = App(name="MyApp")
```

### Routes

Routes define URL endpoints and their handlers:

```python
@app.route('/')
def home():
    return "Welcome to Framework!"

@app.route('/users/<int:user_id>')
def get_user(user_id):
    return f"User ID: {user_id}"
```

### Request Handling

Access request data using the `request` object:

```python
from framework import request

@app.route('/api/data', methods=['POST'])
def handle_data():
    # Get JSON data
    data = request.get_json()
    
    # Get form data
    username = request.form.get('username')
    
    # Get query parameters
    page = request.args.get('page', 1, type=int)
    
    return {"received": data}
```

### Response Objects

Return different types of responses:

```python
from framework import jsonify, redirect, render_template

@app.route('/api/users')
def list_users():
    users = get_users_from_db()
    return jsonify(users)

@app.route('/login')
def login_redirect():
    return redirect('/auth/login')

@app.route('/dashboard')
def dashboard():
    return render_template('dashboard.html', user=current_user)
```

## Working with Data

### Models

Define data models using Framework's ORM:

```python
from framework.db import Model, Column, String, Integer, DateTime

class User(Model):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    username = Column(String(80), unique=True, nullable=False)
    email = Column(String(120), unique=True, nullable=False)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    def __repr__(self):
        return f'<User {self.username}>'
```

### Database Operations

Perform CRUD operations:

```python
# Create
user = User(username='john', email='john@example.com')
user.save()

# Read
user = User.query.filter_by(username='john').first()
all_users = User.query.all()

# Update
user.email = 'newemail@example.com'
user.save()

# Delete
user.delete()
```

## Middleware

Add middleware for cross-cutting concerns:

```python
from framework.middleware import Middleware

class AuthMiddleware(Middleware):
    def process_request(self, request):
        if not request.headers.get('Authorization'):
            return {'error': 'Authentication required'}, 401
    
    def process_response(self, request, response):
        response.headers['X-API-Version'] = '1.0'
        return response

app.add_middleware(AuthMiddleware)
```

## Error Handling

Handle errors gracefully:

```python
@app.errorhandler(404)
def not_found(error):
    return jsonify({'error': 'Resource not found'}), 404

@app.errorhandler(ValidationError)
def validation_error(error):
    return jsonify({'error': str(error)}), 400

# Custom exceptions
class BusinessLogicError(Exception):
    pass

@app.errorhandler(BusinessLogicError)
def business_error(error):
    return jsonify({'error': 'Business logic error'}), 422
```

## Validation

Validate input data:

```python
from framework.validation import Schema, fields

class UserSchema(Schema):
    username = fields.Str(required=True, validate=Length(min=3, max=20))
    email = fields.Email(required=True)
    age = fields.Int(validate=Range(min=18, max=120))

@app.route('/users', methods=['POST'])
def create_user():
    schema = UserSchema()
    try:
        data = schema.load(request.get_json())
        user = User(**data)
        user.save()
        return jsonify(user.to_dict()), 201
    except ValidationError as err:
        return jsonify({'errors': err.messages}), 400
```

## Background Tasks

Execute tasks asynchronously:

```python
from framework.tasks import task

@task
def send_email(to, subject, body):
    # Send email logic here
    pass

@app.route('/send-notification')
def send_notification():
    send_email.delay('user@example.com', 'Hello', 'Welcome!')
    return {'message': 'Email queued'}
```

## Caching

Implement caching for better performance:

```python
from framework.cache import cache

@app.route('/expensive-operation')
@cache.cached(timeout=300)  # Cache for 5 minutes
def expensive_operation():
    # Expensive computation
    result = perform_complex_calculation()
    return jsonify(result)

# Manual cache operations
cache.set('key', 'value', timeout=600)
value = cache.get('key')
cache.delete('key')
```

## Logging

Use structured logging:

```python
import logging
from framework.logging import get_logger

logger = get_logger(__name__)

@app.route('/process-data')
def process_data():
    logger.info('Starting data processing', extra={'user_id': 123})
    
    try:
        result = process_user_data()
        logger.info('Data processing completed', extra={'result_count': len(result)})
        return jsonify(result)
    except Exception as e:
        logger.error('Data processing failed', extra={'error': str(e)})
        raise
```

## Testing

Write tests for your application:

```python
import pytest
from framework.testing import TestClient

@pytest.fixture
def client():
    app.config['TESTING'] = True
    return TestClient(app)

def test_home_page(client):
    response = client.get('/')
    assert response.status_code == 200
    assert b'Welcome' in response.data

def test_create_user(client):
    data = {'username': 'testuser', 'email': 'test@example.com'}
    response = client.post('/users', json=data)
    assert response.status_code == 201
    assert response.json['username'] == 'testuser'
```

## Best Practices

### Code Organization
- Use blueprints for large applications
- Separate business logic from route handlers
- Keep route handlers thin

### Performance
- Use database indexes appropriately
- Implement caching for expensive operations
- Use connection pooling

### Security
- Validate all input data
- Use HTTPS in production
- Implement proper authentication and authorization
- Sanitize output data

### Error Handling
- Use specific exception types
- Log errors with context
- Return meaningful error messages
- Don't expose internal details in production

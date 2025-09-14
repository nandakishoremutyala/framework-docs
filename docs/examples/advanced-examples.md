# Advanced Examples

This section provides advanced examples that demonstrate complex framework usage patterns and real-world scenarios.

## Microservices Architecture Example

Building a microservices-based application:

```python
from framework import Framework
from framework.extensions import DatabaseExtension, CacheExtension
from framework.utils import setup_logger
import asyncio

class UserService:
    def __init__(self, app):
        self.app = app
        self.logger = setup_logger('user_service')
    
    async def create_user(self, user_data):
        # Validate user data
        if not self._validate_user_data(user_data):
            raise ValueError("Invalid user data")
        
        # Save to database
        user_id = await self.app.db.insert('users', user_data)
        
        # Cache user data
        await self.app.cache.set(f"user:{user_id}", user_data, ttl=3600)
        
        # Publish event
        await self.app.events.publish('user.created', {
            'user_id': user_id,
            'user_data': user_data
        })
        
        return user_id
    
    def _validate_user_data(self, data):
        required_fields = ['name', 'email']
        return all(field in data for field in required_fields)

# Main application
app = Framework({
    'service_name': 'user_service',
    'port': 8001
})
app.initialize()

# Add extensions
db_ext = DatabaseExtension('database')
cache_ext = CacheExtension('cache')
db_ext.initialize(app)
cache_ext.initialize(app)

# Initialize service
user_service = UserService(app)

@app.route('/users', methods=['POST'])
async def create_user():
    user_data = app.request.json
    try:
        user_id = await user_service.create_user(user_data)
        return {'user_id': user_id, 'status': 'created'}, 201
    except ValueError as e:
        return {'error': str(e)}, 400

app.run()
```

## Event-Driven Architecture Example

Implementing event-driven patterns:

```python
from framework import Framework
from framework.events import EventBus
import asyncio

class OrderProcessor:
    def __init__(self, event_bus):
        self.event_bus = event_bus
        self.setup_event_handlers()
    
    def setup_event_handlers(self):
        self.event_bus.subscribe('order.created', self.process_order)
        self.event_bus.subscribe('payment.completed', self.fulfill_order)
        self.event_bus.subscribe('order.shipped', self.send_notification)
    
    async def process_order(self, event_data):
        order_id = event_data['order_id']
        print(f"Processing order {order_id}")
        
        # Validate inventory
        if await self.check_inventory(order_id):
            await self.event_bus.publish('inventory.reserved', {
                'order_id': order_id
            })
        else:
            await self.event_bus.publish('order.failed', {
                'order_id': order_id,
                'reason': 'insufficient_inventory'
            })
    
    async def fulfill_order(self, event_data):
        order_id = event_data['order_id']
        print(f"Fulfilling order {order_id}")
        
        # Ship the order
        await self.ship_order(order_id)
        
        await self.event_bus.publish('order.shipped', {
            'order_id': order_id,
            'tracking_number': f"TRK{order_id}"
        })
    
    async def send_notification(self, event_data):
        order_id = event_data['order_id']
        tracking_number = event_data['tracking_number']
        print(f"Sending notification for order {order_id}: {tracking_number}")
    
    async def check_inventory(self, order_id):
        # Simulate inventory check
        await asyncio.sleep(0.1)
        return True
    
    async def ship_order(self, order_id):
        # Simulate shipping process
        await asyncio.sleep(0.2)

# Application setup
app = Framework()
app.initialize()

event_bus = EventBus()
order_processor = OrderProcessor(event_bus)

@app.route('/orders', methods=['POST'])
async def create_order():
    order_data = app.request.json
    order_id = await app.db.insert('orders', order_data)
    
    await event_bus.publish('order.created', {
        'order_id': order_id,
        'order_data': order_data
    })
    
    return {'order_id': order_id, 'status': 'created'}, 201

app.run()
```

## Plugin System Example

Creating a flexible plugin system:

```python
from framework import Framework
from framework.extensions import Extension
import importlib
import os

class PluginManager:
    def __init__(self, app):
        self.app = app
        self.plugins = {}
        self.plugin_dir = 'plugins'
    
    def discover_plugins(self):
        """Automatically discover plugins in the plugins directory."""
        if not os.path.exists(self.plugin_dir):
            return
        
        for filename in os.listdir(self.plugin_dir):
            if filename.endswith('.py') and not filename.startswith('__'):
                plugin_name = filename[:-3]
                self.load_plugin(plugin_name)
    
    def load_plugin(self, plugin_name):
        """Load a plugin by name."""
        try:
            module = importlib.import_module(f'{self.plugin_dir}.{plugin_name}')
            plugin_class = getattr(module, 'Plugin')
            plugin = plugin_class()
            
            if plugin.initialize(self.app):
                self.plugins[plugin_name] = plugin
                plugin.activate()
                print(f"Loaded plugin: {plugin_name}")
            else:
                print(f"Failed to initialize plugin: {plugin_name}")
        except Exception as e:
            print(f"Error loading plugin {plugin_name}: {e}")
    
    def unload_plugin(self, plugin_name):
        """Unload a plugin by name."""
        if plugin_name in self.plugins:
            self.plugins[plugin_name].deactivate()
            del self.plugins[plugin_name]
            print(f"Unloaded plugin: {plugin_name}")

# Example plugin (plugins/analytics.py)
class AnalyticsPlugin(Extension):
    def __init__(self):
        super().__init__('analytics', '1.0.0')
        self.request_count = 0
    
    def initialize(self, framework_instance):
        self.framework = framework_instance
        return True
    
    def activate(self):
        # Add middleware to track requests
        @self.framework.middleware
        def analytics_middleware(request, response):
            self.request_count += 1
            print(f"Total requests: {self.request_count}")
            return response
        
        # Add analytics endpoint
        @self.framework.route('/analytics')
        def analytics():
            return {
                'total_requests': self.request_count,
                'plugin_version': self.version
            }
        
        return True
    
    def deactivate(self):
        # Cleanup would go here
        return True

# Main application
app = Framework()
app.initialize()

# Initialize plugin manager
plugin_manager = PluginManager(app)
plugin_manager.discover_plugins()

app.run()
```

## Real-time Communication Example

WebSocket-based real-time features:

```python
from framework import Framework
from framework.websocket import WebSocketManager
import asyncio
import json

class ChatRoom:
    def __init__(self):
        self.clients = set()
        self.message_history = []
    
    async def add_client(self, websocket):
        self.clients.add(websocket)
        # Send recent message history
        for message in self.message_history[-10:]:
            await websocket.send(json.dumps(message))
    
    async def remove_client(self, websocket):
        self.clients.discard(websocket)
    
    async def broadcast_message(self, message):
        self.message_history.append(message)
        if len(self.message_history) > 100:
            self.message_history.pop(0)
        
        # Broadcast to all connected clients
        disconnected = set()
        for client in self.clients:
            try:
                await client.send(json.dumps(message))
            except:
                disconnected.add(client)
        
        # Remove disconnected clients
        self.clients -= disconnected

app = Framework()
app.initialize()

ws_manager = WebSocketManager(app)
chat_room = ChatRoom()

@ws_manager.route('/chat')
async def chat_handler(websocket, path):
    await chat_room.add_client(websocket)
    try:
        async for message in websocket:
            data = json.loads(message)
            chat_message = {
                'user': data.get('user', 'Anonymous'),
                'message': data.get('message', ''),
                'timestamp': asyncio.get_event_loop().time()
            }
            await chat_room.broadcast_message(chat_message)
    except:
        pass
    finally:
        await chat_room.remove_client(websocket)

@app.route('/chat')
def chat_page():
    return """
    <!DOCTYPE html>
    <html>
    <head><title>Chat Room</title></head>
    <body>
        <div id="messages"></div>
        <input type="text" id="messageInput" placeholder="Type a message...">
        <button onclick="sendMessage()">Send</button>
        
        <script>
            const ws = new WebSocket('ws://localhost:8000/chat');
            const messages = document.getElementById('messages');
            
            ws.onmessage = function(event) {
                const message = JSON.parse(event.data);
                const div = document.createElement('div');
                div.textContent = `${message.user}: ${message.message}`;
                messages.appendChild(div);
            };
            
            function sendMessage() {
                const input = document.getElementById('messageInput');
                ws.send(JSON.stringify({
                    user: 'User',
                    message: input.value
                }));
                input.value = '';
            }
        </script>
    </body>
    </html>
    """

app.run()
```

## Machine Learning Integration Example

Integrating ML models with the framework:

```python
from framework import Framework
from framework.extensions import CacheExtension
import joblib
import numpy as np
import asyncio

class MLModelService:
    def __init__(self, app):
        self.app = app
        self.models = {}
        self.load_models()
    
    def load_models(self):
        """Load pre-trained models."""
        try:
            self.models['classifier'] = joblib.load('models/classifier.pkl')
            self.models['regressor'] = joblib.load('models/regressor.pkl')
            print("Models loaded successfully")
        except Exception as e:
            print(f"Error loading models: {e}")
    
    async def predict(self, model_name, features):
        """Make predictions with caching."""
        cache_key = f"prediction:{model_name}:{hash(str(features))}"
        
        # Check cache first
        cached_result = await self.app.cache.get(cache_key)
        if cached_result:
            return cached_result
        
        # Make prediction
        if model_name not in self.models:
            raise ValueError(f"Model {model_name} not found")
        
        model = self.models[model_name]
        features_array = np.array(features).reshape(1, -1)
        prediction = model.predict(features_array)[0]
        
        result = {
            'model': model_name,
            'prediction': float(prediction),
            'confidence': self._calculate_confidence(model, features_array)
        }
        
        # Cache result
        await self.app.cache.set(cache_key, result, ttl=3600)
        
        return result
    
    def _calculate_confidence(self, model, features):
        """Calculate prediction confidence."""
        if hasattr(model, 'predict_proba'):
            probabilities = model.predict_proba(features)[0]
            return float(max(probabilities))
        return 0.8  # Default confidence

app = Framework()
app.initialize()

# Add caching
cache_ext = CacheExtension('cache')
cache_ext.initialize(app)

# Initialize ML service
ml_service = MLModelService(app)

@app.route('/predict/<model_name>', methods=['POST'])
async def predict(model_name):
    try:
        features = app.request.json.get('features', [])
        if not features:
            return {'error': 'Features required'}, 400
        
        result = await ml_service.predict(model_name, features)
        return result
    except ValueError as e:
        return {'error': str(e)}, 400
    except Exception as e:
        return {'error': 'Prediction failed'}, 500

@app.route('/models')
def list_models():
    return {
        'available_models': list(ml_service.models.keys()),
        'total_models': len(ml_service.models)
    }

app.run()
```

## Distributed Task Queue Example

Implementing background task processing:

```python
from framework import Framework
from framework.queue import TaskQueue
import asyncio
import json

class TaskProcessor:
    def __init__(self, app):
        self.app = app
        self.queue = TaskQueue(app)
        self.register_tasks()
    
    def register_tasks(self):
        """Register available task handlers."""
        self.queue.register_task('send_email', self.send_email_task)
        self.queue.register_task('process_image', self.process_image_task)
        self.queue.register_task('generate_report', self.generate_report_task)
    
    async def send_email_task(self, task_data):
        """Send email task handler."""
        recipient = task_data['recipient']
        subject = task_data['subject']
        body = task_data['body']
        
        print(f"Sending email to {recipient}: {subject}")
        # Simulate email sending
        await asyncio.sleep(2)
        
        return {'status': 'sent', 'recipient': recipient}
    
    async def process_image_task(self, task_data):
        """Image processing task handler."""
        image_path = task_data['image_path']
        operations = task_data.get('operations', [])
        
        print(f"Processing image: {image_path}")
        # Simulate image processing
        await asyncio.sleep(5)
        
        return {
            'status': 'processed',
            'image_path': image_path,
            'operations_applied': operations
        }
    
    async def generate_report_task(self, task_data):
        """Report generation task handler."""
        report_type = task_data['report_type']
        date_range = task_data['date_range']
        
        print(f"Generating {report_type} report for {date_range}")
        # Simulate report generation
        await asyncio.sleep(10)
        
        return {
            'status': 'generated',
            'report_type': report_type,
            'file_path': f'/reports/{report_type}_{date_range}.pdf'
        }

app = Framework()
app.initialize()

# Initialize task processor
task_processor = TaskProcessor(app)

@app.route('/tasks', methods=['POST'])
async def enqueue_task():
    """Enqueue a new task."""
    task_data = app.request.json
    task_type = task_data.get('task_type')
    
    if not task_type:
        return {'error': 'task_type required'}, 400
    
    try:
        task_id = await task_processor.queue.enqueue(task_type, task_data)
        return {
            'task_id': task_id,
            'status': 'enqueued',
            'task_type': task_type
        }, 201
    except Exception as e:
        return {'error': str(e)}, 400

@app.route('/tasks/<task_id>')
async def get_task_status(task_id):
    """Get task status and result."""
    try:
        status = await task_processor.queue.get_task_status(task_id)
        return status
    except Exception as e:
        return {'error': str(e)}, 404

@app.route('/tasks/<task_id>', methods=['DELETE'])
async def cancel_task(task_id):
    """Cancel a pending task."""
    try:
        success = await task_processor.queue.cancel_task(task_id)
        if success:
            return {'status': 'cancelled'}
        else:
            return {'error': 'Task cannot be cancelled'}, 400
    except Exception as e:
        return {'error': str(e)}, 404

# Start background worker
async def start_worker():
    await task_processor.queue.start_worker()

# Run worker in background
asyncio.create_task(start_worker())

app.run()
```

## Next Steps

These advanced examples demonstrate:

- Microservices architecture patterns
- Event-driven programming
- Plugin systems
- Real-time communication
- Machine learning integration
- Distributed task processing

For more information, see:

- [Best Practices](../user-guide/best-practices.md)
- [API Reference](../api-reference/core-api.md)
- [Contributing Guidelines](../contributing/guidelines.md)

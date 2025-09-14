# Configuration

Framework provides flexible configuration options to customize your application's behavior.

## Configuration Files

Framework uses YAML configuration files located in the `config/` directory:

```
config/
├── settings.yml          # Main configuration
├── database.yml          # Database settings
├── logging.yml           # Logging configuration
└── environments/
    ├── development.yml   # Development overrides
    ├── staging.yml       # Staging overrides
    └── production.yml    # Production overrides
```

## Basic Configuration

### Main Settings (`settings.yml`)

```yaml
app:
  name: "My Framework App"
  version: "1.0.0"
  debug: false
  host: "0.0.0.0"
  port: 8000
  secret_key: "your-secret-key-here"

security:
  cors_enabled: true
  cors_origins: ["*"]
  rate_limiting: true
  max_requests_per_minute: 100

features:
  enable_api_docs: true
  enable_metrics: true
  enable_health_check: true
```

### Database Configuration (`database.yml`)

```yaml
default:
  driver: "postgresql"
  host: "localhost"
  port: 5432
  database: "framework_db"
  username: "framework_user"
  password: "secure_password"
  pool_size: 10
  timeout: 30

redis:
  host: "localhost"
  port: 6379
  database: 0
  password: null
```

## Environment-Specific Configuration

### Development (`environments/development.yml`)

```yaml
app:
  debug: true
  log_level: "DEBUG"

database:
  default:
    database: "framework_dev"
    
features:
  enable_debug_toolbar: true
```

### Production (`environments/production.yml`)

```yaml
app:
  debug: false
  log_level: "WARNING"

security:
  cors_origins: ["https://yourdomain.com"]
  
performance:
  enable_caching: true
  cache_ttl: 3600
```

## Environment Variables

Override configuration using environment variables:

```bash
# App settings
export FRAMEWORK_APP_DEBUG=true
export FRAMEWORK_APP_PORT=9000

# Database settings
export FRAMEWORK_DATABASE_HOST=db.example.com
export FRAMEWORK_DATABASE_PASSWORD=secret

# Security settings
export FRAMEWORK_SECRET_KEY=your-production-secret-key
```

## Loading Configuration

Framework automatically loads configuration in this order:

1. Default settings from `settings.yml`
2. Environment-specific overrides
3. Environment variables
4. Command-line arguments

### Programmatic Access

Access configuration in your code:

```python
from framework import config

# Get configuration values
debug_mode = config.get('app.debug')
database_url = config.get('database.default.host')

# Get with default value
cache_ttl = config.get('performance.cache_ttl', 300)

# Get entire section
app_config = config.get_section('app')
```

## Configuration Validation

Framework validates configuration on startup:

```python
from framework.config import ConfigSchema
from marshmallow import fields

class AppConfigSchema(ConfigSchema):
    name = fields.Str(required=True)
    debug = fields.Bool(default=False)
    port = fields.Int(validate=lambda x: 1 <= x <= 65535)

# Register schema
config.register_schema('app', AppConfigSchema)
```

## Dynamic Configuration

Update configuration at runtime:

```python
# Update single value
config.set('app.debug', True)

# Update multiple values
config.update({
    'app.debug': True,
    'app.log_level': 'DEBUG'
})

# Reload from files
config.reload()
```

## Best Practices

### Security
- Never commit sensitive data to version control
- Use environment variables for secrets
- Rotate secret keys regularly

### Organization
- Group related settings together
- Use descriptive names
- Document complex configurations

### Environment Management
- Keep environment configs minimal
- Only override what's necessary
- Test configuration changes

## Configuration Examples

### Microservice Configuration

```yaml
app:
  name: "User Service"
  service_id: "user-svc"

services:
  auth_service:
    url: "http://auth-service:8001"
    timeout: 5
  
  notification_service:
    url: "http://notification-service:8002"
    timeout: 10

circuit_breaker:
  failure_threshold: 5
  recovery_timeout: 60
```

### Multi-Database Setup

```yaml
databases:
  primary:
    driver: "postgresql"
    host: "primary-db.example.com"
    database: "app_primary"
    
  analytics:
    driver: "clickhouse"
    host: "analytics-db.example.com"
    database: "app_analytics"
    
  cache:
    driver: "redis"
    host: "cache.example.com"
```

## Troubleshooting

### Common Issues

**Configuration not loading:**
- Check file paths and permissions
- Verify YAML syntax
- Check environment variable names

**Environment variables not working:**
- Ensure proper naming convention
- Check variable scope and export
- Verify data type conversion

**Validation errors:**
- Review schema definitions
- Check required fields
- Validate data types and constraints

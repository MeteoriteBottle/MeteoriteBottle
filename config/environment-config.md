# Environment Configuration Guide

## Overview

This document outlines the environment configuration settings for the Meteorite Drift Bottle project, covering development, testing, and production environments.

## Environment Variables

### Core Configuration

```env
# .env.example
NODE_ENV=development
PORT=3000
API_VERSION=v1
DEBUG_MODE=true

# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=meteorite_db
DB_USER=admin
DB_PASSWORD=secure_password

# Redis Cache
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=cache_password

# WebSocket
WS_PORT=8080
WS_PATH=/game
```

## Development Setup

### Local Environment

```json
// config/development.json
{
  "server": {
    "host": "localhost",
    "port": 3000,
    "cors": {
      "origin": "*",
      "methods": ["GET", "POST"]
    }
  },
  "game": {
    "tickRate": 60,
    "maxPlayers": 100,
    "debug": true
  }
}
```

## Production Configuration

### Server Settings

```json
// config/production.json
{
  "server": {
    "host": "0.0.0.0",
    "port": 80,
    "cors": {
      "origin": "https://meteoritedriftbottle.com",
      "methods": ["GET", "POST"]
    }
  },
  "game": {
    "tickRate": 60,
    "maxPlayers": 1000,
    "debug": false
  }
}
```

## Game Engine Configuration

### Physics Settings

```typescript
interface PhysicsConfig {
  gravity: number;
  friction: number;
  maxVelocity: number;
  collisionPrecision: number;
}

const defaultPhysics: PhysicsConfig = {
  gravity: 0,
  friction: 0.98,
  maxVelocity: 10,
  collisionPrecision: 2
};
```

## Network Configuration

### WebSocket Settings

```typescript
interface NetworkConfig {
  maxConnections: number;
  timeout: number;
  pingInterval: number;
  pongTimeout: number;
}

const networkDefaults: NetworkConfig = {
  maxConnections: 1000,
  timeout: 5000,
  pingInterval: 30000,
  pongTimeout: 5000
};
```

## Security Configuration

### Authentication Settings

```typescript
interface SecurityConfig {
  jwtSecret: string;
  tokenExpiration: string;
  saltRounds: number;
  rateLimit: {
    windowMs: number;
    max: number;
  };
}
```

## Cache Configuration

### Redis Settings

```typescript
interface CacheConfig {
  host: string;
  port: number;
  password: string;
  ttl: number;
  prefix: string;
}
```

## Logging Configuration

### Winston Logger

```typescript
const loggerConfig = {
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
};
```

## Asset Configuration

### Resource Settings

```typescript
interface AssetConfig {
  basePath: string;
  maxSize: number;
  allowedTypes: string[];
  compression: {
    enabled: boolean;
    quality: number;
  };
}
```

## Database Configuration

### Connection Settings

```typescript
interface DatabaseConfig {
  client: 'pg';
  connection: {
    host: string;
    port: number;
    database: string;
    user: string;
    password: string;
  };
  pool: {
    min: number;
    max: number;
  };
  migrations: {
    tableName: string;
    directory: string;
  };
}
```

## Deployment Configuration

### Docker Settings

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    environment:
      NODE_ENV: production
      PORT: 3000
    ports:
      - "3000:3000"
    depends_on:
      - db
      - redis
```

## Testing Configuration

### Jest Settings

```javascript
// jest.config.js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  setupFiles: ['./test/setup.ts'],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80
    }
  }
};
```

## Best Practices

1. Use environment variables for sensitive data
2. Implement proper error handling
3. Regular configuration review
4. Secure credential management
5. Version control for configs

## Configuration Management

### Loading Configuration

```typescript
class ConfigManager {
  private static instance: ConfigManager;
  private config: any;

  private constructor() {
    this.loadConfig();
  }

  private loadConfig() {
    const env = process.env.NODE_ENV || 'development';
    this.config = require(`./config/${env}.json`);
  }

  static getInstance(): ConfigManager {
    if (!ConfigManager.instance) {
      ConfigManager.instance = new ConfigManager();
    }
    return ConfigManager.instance;
  }

  get(key: string): any {
    return this.config[key];
  }
}
```

## Future Improvements

1. Enhanced security measures
2. Better configuration validation
3. Dynamic configuration updates
4. Configuration monitoring
5. Automated deployment configs
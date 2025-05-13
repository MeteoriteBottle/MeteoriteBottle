# API Reference

## Overview

The Meteorite Drift Bottle API provides endpoints for message handling, game interactions, and real-time communication. This document details the available endpoints, their parameters, and expected responses.

## Base URL

```
https://api.meteoritedriftbottle.com/v1
```

## Authentication

All API requests require authentication using JWT tokens.

```http
Authorization: Bearer <your_jwt_token>
```

## Endpoints

### Meteorite Messages

#### Send Message

```http
POST /messages/send
```

**Parameters**
```json
{
  "content": "string",
  "type": "text|image",
  "tags": ["string"],
  "language": "string"
}
```

**Response**
```json
{
  "messageId": "string",
  "timestamp": "string",
  "status": "sent"
}
```

#### Receive Message

```http
GET /messages/receive
```

**Response**
```json
{
  "messageId": "string",
  "content": "string",
  "type": "text|image",
  "timestamp": "string",
  "tags": ["string"]
}
```

### Game Interactions

#### Get Game State

```http
GET /game/state
```

**Response**
```json
{
  "playerId": "string",
  "position": {
    "x": "number",
    "y": "number"
  },
  "resources": {
    "stardust": "number",
    "energy": "number"
  }
}
```

#### Update Position

```http
POST /game/position
```

**Parameters**
```json
{
  "x": "number",
  "y": "number",
  "velocity": {
    "x": "number",
    "y": "number"
  }
}
```

### Radio Wave Chatroom

#### Join Chat

```http
POST /chat/join
```

**Parameters**
```json
{
  "roomId": "string",
  "username": "string"
}
```

#### Send Chat Message

```http
POST /chat/message
```

**Parameters**
```json
{
  "roomId": "string",
  "content": "string",
  "type": "text|emoji"
}
```

### User Management

#### Create Account

```http
POST /users/create
```

**Parameters**
```json
{
  "username": "string",
  "email": "string",
  "password": "string"
}
```

#### Update Profile

```http
PUT /users/profile
```

**Parameters**
```json
{
  "username": "string",
  "avatar": "string",
  "preferences": {
    "language": "string",
    "notifications": "boolean"
  }
}
```

## WebSocket Events

### Connection

```javascript
const socket = new WebSocket('wss://api.meteoritedriftbottle.com/ws');
```

### Event Types

#### Game Events

```javascript
// Player Movement
socket.emit('move', {
  x: number,
  y: number
});

// Resource Collection
socket.on('resource_collected', {
  type: string,
  amount: number
});
```

#### Chat Events

```javascript
// New Message
socket.emit('chat_message', {
  content: string,
  roomId: string
});

// User Joined
socket.on('user_joined', {
  username: string,
  timestamp: string
});
```

## Error Handling

The API uses standard HTTP status codes:

- 200: Success
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 500: Server Error

**Error Response Format**
```json
{
  "error": {
    "code": "string",
    "message": "string",
    "details": {}
  }
}
```

## Rate Limiting

API requests are limited to:
- 100 requests per minute for regular endpoints
- 60 messages per hour for meteorite sending
- 1000 WebSocket events per minute

## SDK Examples

### JavaScript

```javascript
const MeteoriteDriftBottle = require('meteorite-drift-bottle-sdk');

const client = new MeteoriteDriftBottle({
  apiKey: 'your_api_key',
  environment: 'production'
});

// Send a message
await client.messages.send({
  content: 'Hello from the stars!',
  type: 'text'
});

// Join a chat room
await client.chat.join({
  roomId: 'cosmic-lounge',
  username: 'stargazer'
});
```

## Best Practices

1. Implement exponential backoff for retries
2. Cache responses when appropriate
3. Use WebSocket for real-time features
4. Handle rate limits gracefully
5. Validate input before sending

## Support

For API support, please contact:
- Email: api@meteoritedriftbottle.com
- Discord: MeteoriteDriftBottle#tech-support

## Changelog

### v1.0.0 (2024-01)
- Initial API release
- Basic message and game functionality
- WebSocket support

### v1.1.0 (2024-02)
- Enhanced chat features
- Improved error handling
- New game endpoints
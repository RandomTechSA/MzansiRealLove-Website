# Mzansi Real Love - API Documentation

## Authentication API

### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
    "email": "string",
    "password": "string",
    "name": "string",
    "dateOfBirth": "string (ISO-8601)",
    "gender": "string"
}
```

### Login
```http
POST /api/auth/login
Content-Type: application/json

{
    "email": "string",
    "password": "string"
}
```

## Profile API

### Get Profile
```http
GET /api/profile/{userId}
Authorization: Bearer {token}
```

### Update Profile
```http
PUT /api/profile/{userId}
Authorization: Bearer {token}
Content-Type: application/json

{
    "name": "string",
    "bio": "string",
    "interests": ["string"],
    "photos": ["string (URL)"],
    "location": {
        "latitude": number,
        "longitude": number
    }
}
```

## Matching API

### Get Matches
```http
GET /api/matches
Authorization: Bearer {token}
Query Parameters:
    - maxDistance: number (km)
    - minAge: number
    - maxAge: number
    - limit: number
    - offset: number
```

### Like Profile
```http
POST /api/matches/like/{userId}
Authorization: Bearer {token}
```

## Chat API

### Get Conversations
```http
GET /api/chats
Authorization: Bearer {token}
```

### Get Messages
```http
GET /api/chats/{chatId}/messages
Authorization: Bearer {token}
Query Parameters:
    - limit: number
    - before: string (message ID)
```

### Send Message
```http
POST /api/chats/{chatId}/messages
Authorization: Bearer {token}
Content-Type: application/json

{
    "content": "string",
    "type": "TEXT|IMAGE|VOICE|VIDEO"
}
```

## Events API

### Create Event
```http
POST /api/events
Authorization: Bearer {token}
Content-Type: application/json

{
    "title": "string",
    "description": "string",
    "date": "string (ISO-8601)",
    "location": {
        "latitude": number,
        "longitude": number
    },
    "maxParticipants": number,
    "isPrivate": boolean
}
```

### Get Events
```http
GET /api/events
Authorization: Bearer {token}
Query Parameters:
    - nearLat: number
    - nearLng: number
    - radius: number (km)
    - startDate: string (ISO-8601)
    - endDate: string (ISO-8601)
```

## Media API

### Upload Media
```http
POST /api/media/upload
Authorization: Bearer {token}
Content-Type: multipart/form-data

file: binary
```

### Get Media
```http
GET /api/media/{mediaId}
Authorization: Bearer {token}
```

## WebRTC Signaling API

### Create Call
```http
POST /api/calls
Authorization: Bearer {token}
Content-Type: application/json

{
    "targetUserId": "string",
    "type": "VIDEO|VOICE"
}
```

### Send ICE Candidate
```http
POST /api/calls/{callId}/ice
Authorization: Bearer {token}
Content-Type: application/json

{
    "candidate": "string",
    "sdpMid": "string",
    "sdpMLineIndex": number
}
```

## Error Responses

### 400 Bad Request
```json
{
    "error": "string",
    "message": "string",
    "details": {}
}
```

### 401 Unauthorized
```json
{
    "error": "Unauthorized",
    "message": "Invalid or expired token"
}
```

### 403 Forbidden
```json
{
    "error": "Forbidden",
    "message": "Insufficient permissions"
}
```

### 404 Not Found
```json
{
    "error": "NotFound",
    "message": "Resource not found"
}
```

### 429 Too Many Requests
```json
{
    "error": "TooManyRequests",
    "message": "Rate limit exceeded",
    "retryAfter": number
}
```

### 500 Server Error
```json
{
    "error": "ServerError",
    "message": "Internal server error",
    "requestId": "string"
}
```

## WebSocket Events

### Connection
```javascript
ws://api.mzansireallove.com/ws?token={token}
```

### Message Types

#### Chat Message
```json
{
    "type": "CHAT_MESSAGE",
    "data": {
        "chatId": "string",
        "senderId": "string",
        "content": "string",
        "timestamp": "string (ISO-8601)"
    }
}
```

#### Typing Indicator
```json
{
    "type": "TYPING",
    "data": {
        "chatId": "string",
        "userId": "string",
        "isTyping": boolean
    }
}
```

#### Match Notification
```json
{
    "type": "NEW_MATCH",
    "data": {
        "matchId": "string",
        "userId": "string",
        "timestamp": "string (ISO-8601)"
    }
}
```

#### Call Signal
```json
{
    "type": "CALL_SIGNAL",
    "data": {
        "callId": "string",
        "signal": "string",
        "type": "OFFER|ANSWER|ICE_CANDIDATE"
    }
}
```

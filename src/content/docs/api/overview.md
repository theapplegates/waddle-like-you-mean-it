---
title: "API Overview"
description: "Complete overview of our API endpoints and methods."
category: "api"
order: 1
lastUpdated: 2024-01-15
---

# API Overview

Complete reference for all available API methods and endpoints.

## Authentication

All API requests require authentication using your API key:

```javascript
const headers = {
  'Authorization': `Bearer ${apiKey}`,
  'Content-Type': 'application/json'
};
```

## Base URL

All API endpoints are relative to:

```
https://api.example.com/v1/
```

## Core Methods

### getData()

Retrieve data from the API.

```typescript
async getData(options?: {
  limit?: number;
  offset?: number;
  filter?: Record<string, any>;
}): Promise<DataResponse>
```

**Parameters:**
- `limit` (optional): Maximum number of results (default: 10)
- `offset` (optional): Number of results to skip (default: 0)
- `filter` (optional): Filter criteria object

**Example:**

```javascript
const data = await app.getData({
  limit: 20,
  filter: { status: 'active' }
});
```

### createData()

Create new data entry.

```typescript
async createData(data: CreateDataRequest): Promise<DataResponse>
```

**Example:**

```javascript
const newData = await app.createData({
  name: 'New Item',
  description: 'Item description',
  category: 'example'
});
```

### updateData()

Update existing data entry.

```typescript
async updateData(id: string, data: UpdateDataRequest): Promise<DataResponse>
```

**Example:**

```javascript
const updated = await app.updateData('123', {
  name: 'Updated Name',
  status: 'completed'
});
```

### deleteData()

Delete data entry.

```typescript
async deleteData(id: string): Promise<void>
```

**Example:**

```javascript
await app.deleteData('123');
```

## Response Format

All successful API responses follow this format:

```typescript
interface ApiResponse<T> {
  success: true;
  data: T;
  meta?: {
    total?: number;
    page?: number;
    limit?: number;
  };
}
```

## Error Handling

Error responses follow this format:

```typescript
interface ApiError {
  success: false;
  error: {
    code: string;
    message: string;
    details?: any;
  };
}
```

### Common Error Codes

- `AUTH_REQUIRED`: Authentication required
- `INVALID_REQUEST`: Request validation failed
- `NOT_FOUND`: Resource not found
- `RATE_LIMITED`: Rate limit exceeded
- `SERVER_ERROR`: Internal server error

## Rate Limits

API requests are limited to:

- **1000 requests per hour** for authenticated users
- **100 requests per hour** for unauthenticated requests

Rate limit headers are included in all responses:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1640995200
```

## SDKs and Libraries

Official SDKs are available for:

- [JavaScript/TypeScript](/docs/api/javascript-sdk)
- [Python](/docs/api/python-sdk)
- [Go](/docs/api/go-sdk)
---
title: "Basic Usage Examples"
description: "Simple examples to get you started with basic functionality."
category: "examples"
order: 1
lastUpdated: 2025-07-01
---

## Hello World

The simplest possible example:

```javascript
import { initialize } from "our-package";

// Initialize
const app = initialize({
  apiKey: "your-api-key",
  environment: "development",
});

// Make your first call
const result = await app.getData();
console.log("Hello World:", result);
```

## Fetching Data

Retrieve data with filtering and pagination:

```javascript
// Get first 10 active items
const activeItems = await app.getData({
  limit: 10,
  filter: { status: "active" },
});

console.log(`Found ${activeItems.length} active items`);

// Get next page
const nextPage = await app.getData({
  limit: 10,
  offset: 10,
  filter: { status: "active" },
});
```

## Creating Data

Create new entries:

```javascript
// Create a single item
const newItem = await app.createData({
  name: "My First Item",
  description: "This is a test item",
  category: "example",
  tags: ["test", "demo"],
});

console.log("Created item:", newItem.id);

// Create multiple items
const items = [
  { name: "Item 1", category: "type-a" },
  { name: "Item 2", category: "type-b" },
  { name: "Item 3", category: "type-a" },
];

for (const item of items) {
  await app.createData(item);
}
```

## Updating Data

Update existing entries:

```javascript
// Update a single field
await app.updateData("item-123", {
  status: "completed",
});

// Update multiple fields
await app.updateData("item-456", {
  name: "Updated Name",
  description: "Updated description",
  lastModified: new Date(),
});
```

## Error Handling

Handle errors gracefully:

```javascript
try {
  const data = await app.getData();
  console.log("Success:", data);
} catch (error) {
  if (error.code === "AUTH_REQUIRED") {
    console.error("Please check your API key");
  } else if (error.code === "RATE_LIMITED") {
    console.error("Too many requests, please wait");
  } else {
    console.error("Unexpected error:", error.message);
  }
}
```

## Working with Filters

Advanced filtering examples:

```javascript
// Text search
const searchResults = await app.getData({
  filter: {
    name: { contains: "search term" },
  },
});

// Date range
const recentItems = await app.getData({
  filter: {
    createdAt: {
      gte: new Date("2024-01-01"),
      lte: new Date("2024-12-31"),
    },
  },
});

// Multiple conditions
const complexFilter = await app.getData({
  filter: {
    status: "active",
    category: { in: ["type-a", "type-b"] },
    priority: { gte: 5 },
  },
});
```

## Batch Operations

Perform operations on multiple items:

```javascript
// Get all items in a category
const categoryItems = await app.getData({
  filter: { category: "important" },
});

// Update all items in batch
for (const item of categoryItems) {
  await app.updateData(item.id, {
    processed: true,
    processedAt: new Date(),
  });
}

console.log(`Processed ${categoryItems.length} items`);
```

## Real-time Updates

Listen for real-time updates:

```javascript
// Subscribe to changes
app.subscribe("data-updated", (event) => {
  console.log("Data updated:", event.data);
});

// Unsubscribe when done
app.unsubscribe("data-updated");
```

## Next Steps

Ready for more advanced examples? Check out:

- [Advanced Configuration](/docs/examples/advanced-config)
- [Integration Patterns](/docs/examples/integrations)
- [Performance Optimization](/docs/examples/performance)

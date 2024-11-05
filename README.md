# API Integration Solutions

This repository contains solutions for API integration technical assessments.

## Table of Contents

- [Task 1 : HubSpot Deals API Integration](#hubspot-deals-api-integration)
- [Task 2 : API Error Handling Implementation](#api-error-handling-implementation)
- [Task 3 : Quick SQL Query](#quick-sql-query)
- [Task 4 : Rate Limiting](#rate-limiting)

## Task 1 : HubSpot Deals API Integration

A Node.js application demonstrating HubSpot Deals API integration with MongoDB synchronization.

### Demo Video

[![HubSpot Deals API Demo](https://i.ytimg.com/vi/UJAnjpPIEkw/default.jpg)](https://youtu.be/UJAnjpPIEkw)

### Features

- Docker containerization
- Mock deal generation
- High-priority deal filtering
- Real-time MongoDB synchronization

### Repository

[View Source Code](https://github.com/moelahmady/9H-HubSpot-Deals-API-Integration.git)

## API Error Handling Implementation

Demonstration of robust error handling in API integrations using Node.js and Axios.

### Original Implementation

```javascript
const axios = require("axios");

async function fetchData(url) {
  const response = await axios.get(url);
  const data = response.data;

  data.forEach((item) => {
    console.log(item.name.toUpperCase());
    console.log(item.value * 2);
  });
}

const apiUrl = "https://some-api.com/data";
fetchData(apiUrl);
```

### Enhanced Implementation

```javascript
const axios = require("axios");

async function fetchData(url) {
  try {
    const response = await axios.get(url);
    const data = response.data;

    // Validate if data is an array and not empty
    if (!Array.isArray(data) || data.length === 0) {
      console.error("Error: Invalid or empty data received from the API.");
      return; // Or throw an error depending on how you want to handle it
    }

    data.forEach((item) => {
      // Validate individual item properties before using them
      if (
        item &&
        item.name &&
        typeof item.name === "string" &&
        item.value &&
        typeof item.value === "number"
      ) {
        console.log(item.name.toUpperCase());
        console.log(item.value * 2);
      } else {
        console.error("Error: Invalid item data:", item); // Log the invalid item for debugging
      }
    });
  } catch (error) {
    // Handle API request errors
    console.error("Error fetching data:", error.message); // Log the error message
    // Optionally, you can examine error.response for more details about the API error
    if (error.response) {
      console.error("API Error Status:", error.response.status);
      console.error("API Error Data:", error.response.data);
    }
  }
}

const apiUrl = "https://some-api.com/data";

fetchData(apiUrl);
```

### Improvements Made

- Added comprehensive error handling
- Input validation
- Type checking
- Structured error logging
- Modular code organization
- Documentation

## Task 3 : Quick SQL Query

Query to to retrieve all entries from a table named integrations_log where status = 'error' and timestamp is within the last 7 days.

### MSSQL

```sql
SELECT * FROM integrations_log WHERE status = 'error' AND timestamp >= DATEADD(day, -7, GETDATE());
```

### MySQL

```sql
SELECT * FROM integrations_log WHERE status = 'error' AND timestamp >= NOW() - INTERVAL 7 DAY;
```

### PostgreSQL

```sql
SELECT * FROM integrations_log WHERE status = 'error' AND timestamp >= NOW() - INTERVAL '7 days';
```

### SQLite

```sql
SELECT * FROM integrations_log WHERE status = 'error' AND timestamp >= DATETIME('now', '-7 days');
```

## Task 4 : How you would handle rate limits for an API call that needs to process high volumes of data.

- Implement exponential backoff to manage the retry logic and avoid overwhelming the API provider.
- Implement circuit breaker pattern to prevent cascading failures when the API is under heavy load.
- Implement rate limiting middleware to control the number of requests sent to the API.

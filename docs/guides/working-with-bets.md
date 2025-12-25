# Working with Bets

## Overview

This guide explains how to retrieve and work with bet data using the available API endpoints.
It covers common use cases for listing bets and retrieving individual bet details, and explains how to interpret the returned data.

The guide is intended for backend services and reporting tools that require access to bet information.

## When to Use Each Endpoint

Choose the appropriate endpoint based on the use case or required level of detail.

**Retrieve Bets by Status (`GET /api/v1/bets`)**

Use this endpoint to list all bets for a selected user or time period. It is suitable for overviews, dashboards, and reporting where multiple bets are required.

**Retrieve a Bet by ID (`GET /api/v1/bets/{betId}`)**

Use this endpoint to get details of a single bet. It is suitable for inspecting an individual bet, including its current status and resolution state.

## Retrieving Bets by Status

The `GET /api/v1/bets` endpoint lists bets for a specific user and can be filtered by status and date range.

**Typical Use Cases**

- Display active bets for a user in a dashboard
- Retrieve settled bets for reporting
- Filter bets within a date range for analytics

**Key Parameters**

- `userId` (required) – unique user identifier
- `status` (optional) – filter by bet status: OPEN, SETTLED, CANCELLED
- `fromDate` / `toDate` (optional) – date range for filtering (ISO 8601)

**Example Request**

```bash
curl -X GET "https://api.example.com/v1/bets?userId=123abc&status=SETTLED&fromDate=2025-12-20&toDate=2025-12-21"
-H "Authorization: Bearer <YOUR_TOKEN>"
```

**Notes**

- Omitting `status` returns all bets for the user
- Results are returned in descending order by `placedAt`
- `currency` field uses ISO 4217 codes (EUR, RSD)

## Retrieving a Bet by ID

The `GET /api/v1/bets/{betId}` endpoint lists details of a single bet by unique bet ID.

**Typical Use Cases**

- Inspect a specific bet to check its status
- Validate bet resolution or cancellation
- Investigate issues related to a single wager

**Key Parameters**

- `betId` (required) – unique identifier of the bet

**Example Request**

```bash
curl -X GET "https://api.example.com/v1/bets/b123"
-H "Authorization: Bearer <YOUR_TOKEN>"
```

**Notes**

- The endpoint returns a single bet, so sorting does not apply
- `currency` field uses ISO 4217 codes (EUR, RSD)
- `status` field indicates the current state of the bet: OPEN, SETTLED, or CANCELLED

## Understanding Bet Data

When you retrieve bet information, each bet object includes the following key fields:

- `betId` – Unique bet identifier
- `userId` – Unique user identifier
- `amount` – Bet stake
- `currency` – Bet amount currency, ISO codes such as EUR or RSD
- `status` – Current bet state; possible values:
  - `OPEN`: Bet is placed and active
  - `SETTLED`: Bet is resolved and outcome is finalized
  - `CANCELLED`: Bet was cancelled and will not be settled
- `placedAt` – Bet placement timestamp (ISO 8601 format)

**Usage Notes**

- Use `status` to filter bets for dashboards and reporting
- Use `placedAt` for sorting or date-range queries
- Use `currency` for financial calculations or aggregations

## Common Scenarios

### List All Active Bets for a User

- Retrieve all bets with status `OPEN` for a specific user
- Useful for dashboards or monitoring active wagers

### Generate Settled Bets Report by Date Range

- Retrieve bets with status `SETTLED` within a date range
- Suitable for analytics or reporting purposes

### Inspect a Single Bet

- Use the `GET /api/v1/bets/{betId}` endpoint
- Useful for verifying bet resolution or investigating discrepancies

## Error Handling

When using these endpoints, you may encounter:

- **400 Bad Request**
  - Example: missing or invalid `betId` or incorrectly formatted dates
  - Action: validate parameters before making the request

- **401 Unauthorized**
  - Example: missing or invalid authentication token
  - Action: ensure a valid bearer token is included

- **404 Not Found**
  - Example: no bets found for the specified criteria
  - Action: verify user ID, bet ID, or query filters

## Related Resources

- **Bet Entity Reference** – full reference for bet fields, statuses, and relationships
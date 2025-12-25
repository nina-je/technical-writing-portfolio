# Retrieve Bet by ID

## Overview

This endpoint returns details of a single bet identified by its unique ID. It is intended for backend services and reporting tools that require access to details of a single bet. The response includes basic bet details and the current bet status.

## Endpoint

GET /api/v1/bets/{betId}

## Authentication

This endpoint requires authentication. Requests must include a bearer token in the `Authorization` header.

## Request Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| betId | string | yes | Unique bet identifier. |

## Query Parameters

This endpoint does not require query parameters.

## Request Example

```
curl -X GET "https://api.example.com/v1/bets/b123"
-H Authorization: Bearer <YOUR_TOKEN>
```

## Response

```
HTTP/1.1 200 OK
Content-Type: application/json
```

**Example Response**

```
{
  "betId": "b123",
  "userId": "123abc",
  "amount": 50.0,
  "currency": "EUR",
  "status": "SETTLED",
  "placedAt": "2025-12-23T14:30:00Z"
}
```

**Response Fields**

| Field | Type | Description |
|-------|------|-------------|
| betId | string | Unique bet identifier. |
| userId | string | Unique user identifier. |
| amount | number | Bet amount. |
| currency | string (ISO 4217) | Bet amount currency. Allowed values: EUR, RSD. |
| status | string (enum) | Current bet status. Allowed values: OPEN, SETTLED, CANCELLED. |
| placedAt | string (ISO 8601) | Bet placement timestamp. |

## Error Responses

**400 Bad Request**

Invalid or missing 'betId'.
```
{
  "error": {
    "code": 400,
    "message": "Invalid or missing 'betId'."
  }
}
```

**401 Unauthorized**

Missing or invalid authentication token.
```
{
  "error": {
    "code": 401,
    "message": "Missing or invalid authentication token."
  }
}
```

**404 Not Found**

No bets found for the specified bet ID.
```
{
  "error": {
    "code": 404,
    "message": "No bets found for the specified bet ID."
  }
}
```

## Notes and Constraints

- The `currency` response field uses ISO 4217 currency code.
- The `status` response field uses enum values. Allowed values are 'OPEN', 'SETTLED', and 'CANCELLED'.
- The `placedAt` response field uses ISO 8601 format.

## Related Resources

- Authentication
- Bet entity reference
- Retrieve bets by status endpoint
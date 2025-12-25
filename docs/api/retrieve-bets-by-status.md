# Retrieve Bets by Status

## Overview

This endpoint returns a list of bets filtered by status. It is intended for backend services and reporting tools that require access to bet information for a specific user or time period. The response includes basic bet details and the current bet status.

## Endpoint

GET /api/v1/bets

## Authentication

This endpoint requires authentication. Requests must include a bearer token in the `Authorization` header.

## Request Parameters

This endpoint does not require path parameters. Filtering is performed using query parameters.

## Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| userId | string | yes | Unique user identifier. |
| status | string (enum) | no | Bet status. Allowed values: OPEN, SETTLED, CANCELLED. If omitted, all user bets are returned. |
| fromDate | string (ISO 8601) | no | Start date for filtering bets by placement time. |
| toDate | string (ISO 8601) | no | End date for filtering bets by placement time. |

## Request Example

```curl -X GET "https://api.example.com/v1/bets?userId=123abc&status=SETTLED&fromDate=2025-12-20&toDate=2025-12-21"
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
  "bets": [
    {
      "betId": "b123",
      "userId": "123abc",
      "amount": 50.0,
      "currency": "EUR",
      "status": "SETTLED",
      "placedAt": "2025-06-01T14:30:00Z"
    },
    {
      "betId": "b124",
      "userId": "123abc",
      "amount": 20.0,
      "currency": "EUR",
      "status": "OPEN",
      "placedAt": "2025-12-10T09:15:00Z"
    }
  ]
}
```

**Response Fields**

The response body contains a list of `Bet` objects.
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

Invalid 'fromDate' format.
```
{
  "error": {
    "code": 400,
    "message": "Invalid 'fromDate' format. Expected ISO 8601 date."
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

No bets found for the specified user and status.
```
{
  "error": {
    "code": 404,
    "message": "No bets found for the specified user and status."
  }
}
```
## Notes and Constraints

- The `status` query parameter is optional. If not set, the endpoint returns all bets for the specified user.
- The `status` query parameter and the `status` response field use the same enum values. Allowed values are 'OPEN', 'SETTLED', and 'CANCELLED'.
- `fromDate` and `toDate` query parameters must be provided in ISO 8601 format.
- `fromDate` must be earlier than or equal to `toDate`.
- Results are returned in descending order by `placedAt` value.
- The `currency` response field uses ISO 4217 currency code. Allowed values are 'EUR', and 'RSD'.


## Related Resources

- Authentication
- Bet entity reference
- Retrieve bet by ID endpoint
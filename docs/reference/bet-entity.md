# Bet Entity Reference

## Overview
The Bet entity represents a single wager placed by a user. It is a core entity used in betting, settlement, reporting, and auditing.

A Bet is created when a wager is placed and remains associated with the user throughout its lifecycle. The bet status indicates its current state.

## Entity Attributes
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| betId | string | yes | Unique bet identifier. |
| userId | string | yes | Unique user identifier. |
| amount | number | yes | Bet amount. |
| currency | string (ISO 4217) | yes | Bet amount currency. |
| status | string (enum) | yes | Current bet status. |
| placedAt | string (ISO 8601) | yes | Bet placement timestamp. |

## Status Values
| Value | Description |
|-------|-------------|
| OPEN | The bet is placed and active. |
| SETTLED | The bet is resolved and the outcome finalized. |
| CANCELLED | The bet is cancelled and will not be settled. |

## Currency Codes
| Code | Description |
|------|-------------|
| EUR | Euro |
| RSD | Serbian Dinar |

## Entity Lifecycle
After creation, a bet progresses through various internal states until it is resolved. See the **Status Values** table for allowed statuses.

## Data Constraints
- `betId` must be unique and immutable for the bet lifecycle.
- `userId` must be unique and reference an existing user.
- `currency` must use ISO 4217 format. Allowed values are defined in the **Currency Codes** table.
- `status` must use one of the enum values defined in the **Status Values** table.
- `placedAt` must use ISO 8601 format.

## Relationships
- **Transaction (One-to-One)** – Each bet is linked to a financial transaction that records the stake being deducted from the user's balance.
- **User (Many-to-One)** – Each user can place multiple bets, but each bet belongs to a single user.
- **Selection (Many-to-One)** – Multiple bets from different users can be placed on a single selection.
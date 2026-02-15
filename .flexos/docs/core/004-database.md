---
id: database
title: 'Database Schema & Data Models'
description: 'Detailed specification of the Firestore collections, fields, and relationships that form the data layer for the IRINA project.'
type: doc
subtype: core
status: draft
sequence: 4
tags:
  - database
  - firestore
  - architecture
createdAt: '2023-10-27T10:00:00Z'
updatedAt: '2023-10-27T10:00:00Z'
---

## 1. Introduction

This document outlines the database schema for the IRINA project, which will be implemented using Google Firebase's Firestore. Firestore is a NoSQL, document-based database that provides the flexibility and scalability required for this application. The schema is designed to be efficient, secure, and supportive of all core application features. It consists of three primary top-level collections: `users`, `payment_items`, and `transactions`.

## 2. Collection: `users`

This collection stores all information related to a registered creator's account and public profile.

*   **Document ID:** The `uid` provided by Firebase Authentication.
*   **Description:** Each document represents a single user. This collection is the central source of truth for user identity and profile customization.
*   **Firestore Path:** `/users/{userId}`

### Fields

| Field Name            | Type                 | Required | Description                                                                                                                               |
| --------------------- | -------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `email`               | String (Email)       | True     | The user's login email address. This is primarily for authentication and communication.                                                     |
| `username`            | String               | True     | A unique, URL-safe string chosen by the user at signup. Used for their public profile URL (e.g., `/[username]`). Must be globally unique. |
| `display_name`        | String               | False    | The user's public-facing name, displayed on their profile page. Defaults to the username if not set.                                      |
| `profile_picture_url` | String (URL)         | False    | A URL pointing to the user's profile image, likely hosted in Firebase Storage.                                                            |
| `bio`                 | String               | False    | A short biography or description displayed on the public profile. Limited to ~250 characters.                                             |
| `social_links`        | Array of Maps        | False    | An array of social media links. Each map contains `{ platform: 'instagram', url: 'https://...' }`.                                       |
| `stripe_account_id`   | String               | False    | The ID of the connected Stripe Connect Express account (e.g., `acct_...`). This is critical for processing payouts.                      |
| `created_at`          | Timestamp            | True     | The server-side timestamp of when the user account was created.                                                                           |

### Security Rules
*   Users can only read and write to their own document (`/users/{userId}`).
*   Public profile fields (`username`, `display_name`, `profile_picture_url`, `bio`, `social_links`) can be read by anyone.
*   Sensitive fields like `email` and `stripe_account_id` are only readable by the authenticated owner.

## 3. Collection: `payment_items`

This collection stores every payment or donation item created by a user.

*   **Document ID:** A randomly generated unique ID.
*   **Description:** These documents represent the individual 'products' a creator offers, such as a lesson, a service, or a donation tier.
*   **Firestore Path:** `/payment_items/{itemId}`

### Fields

| Field Name    | Type      | Required | Description                                                                                                      |
| ------------- | --------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| `user_id`     | String    | True     | The UID of the user who owns this item. This acts as a foreign key to the `users` collection.                      |
| `title`       | String    | True     | The public title of the item (e.g., "30-Minute Guitar Lesson").                                                  |
| `description` | String    | False    | A detailed description of the item. Can support rich text/markdown in the future.                                |
| `amount`      | Number    | True     | The price of the item in the smallest currency unit (e.g., cents for USD) to avoid floating-point issues.      |
| `currency`    | String    | True     | The three-letter ISO currency code (e.g., "USD", "EUR").                                                       |
| `image_url`   | String (URL) | False    | A URL to an optional image for the item, hosted in Firebase Storage.                                             |
| `type`        | String    | True     | A category for the item (e.g., 'service', 'donation', 'music').                                                  |
| `is_active`   | Boolean   | True     | Determines if the item is visible on the public profile and available for purchase. Defaults to `true`.          |
| `created_at`  | Timestamp | True     | The server-side timestamp of when the item was created.                                                          |
| `updated_at`  | Timestamp | True     | The server-side timestamp of the last modification.                                                              |

### Security Rules & Indexes
*   Users can only create, read, update, and delete items where `request.auth.uid == resource.data.user_id`.
*   Public reads are allowed for documents where `is_active == true` to populate the profile page.
*   An index must be created on `(user_id, is_active)` for efficient querying of a user's active items.

## 4. Collection: `transactions`

This collection serves as an immutable ledger of all successful payments processed through the platform.

*   **Document ID:** The Stripe Charge ID (`ch_...`) for guaranteed uniqueness and easy reconciliation.
*   **Description:** A new document is created in this collection via a secure backend function (e.g., Firebase Cloud Function) triggered by a Stripe webhook for a successful payment.
*   **Firestore Path:** `/transactions/{transactionId}`

### Fields

| Field Name        | Type   | Required | Description                                                                                                   |
| ----------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------- |
| `user_id`         | String | True     | The UID of the user who received the payment. Foreign key to the `users` collection.                          |
| `payment_item_id` | String | True     | The ID of the `payment_item` that was purchased. Foreign key to the `payment_items` collection.               |
| `amount`          | Number | True     | The final amount of the transaction in the smallest currency unit.                                            |
| `currency`        | String | True     | The three-letter ISO currency code of the transaction.                                                        |
| `payer_email`     | String | False    | The email address of the customer who made the payment, captured from the Stripe transaction.                 |
| `payer_name`      | String | False    | The name of the customer, if provided during checkout.                                                        |
| `status`          | String | True     | The status of the transaction (e.g., 'succeeded', 'pending', 'failed'). Typically will only store 'succeeded'. |
| `stripe_charge_id`| String | True     | The unique identifier for the charge from Stripe's API. Serves as the primary key.                            |
| `created_at`      | Timestamp | True     | The server-side timestamp of when the transaction occurred.                                                   |

### Security Rules & Indexes
*   This collection is write-only from the backend (Cloud Functions). Client-side writes are disallowed.
*   Users can only read transactions where `request.auth.uid == resource.data.user_id`.
*   An index must be created on `(user_id, created_at)` to allow users to efficiently query their own payment history.
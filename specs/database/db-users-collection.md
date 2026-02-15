---
id: db-users-collection
title: Database: Users Collection
description: Specification for the core 'users' data collection, storing creator account details.
type: spec
subtype: database
status: draft
sequence: 400
tags: data, backend, firebase, core
relatesTo: users, secure-auth, profile-customization, account-settings
createdAt: 2023-10-27T10:00:00Z
updatedAt: 2023-10-27T10:00:00Z
---
# Database: Users Collection

This specification details the `users` collection, which serves as the foundational data store for all creator accounts within the IRINA platform. This collection holds critical information necessary for authentication, public profile display, and linking to other user-generated content and financial data.

Each document in the `users` collection represents a unique creator and is identified by a `id` field, which will correspond to the Firebase Authentication UID. The `email` field stores the user's primary email address for login and communication, while a unique `username` is crucial for generating personalized public profile URLs (e.g., `/irina`).

Public-facing profile elements include `display_name` (the name shown on their public page), `profile_picture_url` (a link to their avatar), `bio` (a rich-text field for their personal story or services), and `social_links` (an array of objects containing platform names and URLs for their social media presence). These fields are directly editable via the 'Account & Payout Settings' feature.

For payment processing, the `stripe_account_id` field stores the unique identifier for the creator's connected Stripe Express account, enabling direct payouts. Finally, `created_at` records the timestamp of account creation, providing essential metadata for account management and analytics. This collection is central to establishing a creator's identity and enabling their operations on IRINA.
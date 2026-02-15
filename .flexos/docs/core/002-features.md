---
id: features
title: 'Features & Requirements'
description: 'A detailed breakdown of the core features, their functionality, and associated user stories for the IRINA project.'
type: doc
subtype: core
status: draft
sequence: 2
tags:
  - features
  - product
  - mvp
createdAt: '2023-10-27T10:00:00Z'
updatedAt: '2023-10-27T10:00:00Z'
---

## 1. Introduction

This document outlines the functional requirements for the IRINA platform's Minimum Viable Product (MVP). The features are prioritized to deliver the core value proposition as quickly as possible: enabling creators to create a personal profile and accept payments. Features are categorized into P0 (Must-Have for launch) and P1 (High-Priority for post-launch).

## 2. P0: Core Features (MVP Launch)

### 2.1. Create & Manage Payment/Donation Items (`create-payment-items`)

This is the central feature of IRINA. It allows creators to define specific, purchasable items that will be displayed on their public profile.

*   **Functionality:**
    *   A dedicated 'My Items' page (`/dashboard/items`) will list all created items.
    *   A 'Create New Item' button will open a form (`/dashboard/items/new`) with the following fields:
        *   **Title:** A clear, concise name (e.g., "1-hour English Lesson").
        *   **Description:** A rich-text enabled field for detailed information, allowing for bolding, italics, and lists.
        *   **Amount:** A numerical input for the price. This will be stored in cents to avoid floating-point errors.
        *   **Image:** An optional image upload to visually represent the item.
        *   **Type:** A category selector (e.g., 'Service', 'Donation', 'Music') for internal organization.
        *   **Status:** A toggle to set the item as 'Active' (visible on the public profile) or 'Inactive' (hidden).
    *   Each saved item generates a unique, shareable URL that leads directly to its checkout page (e.g., `/pay/[item-id]`).
    *   Creators can edit or delete existing items from the 'My Items' list.

*   **User Stories:**
    *   As Irina, I want to create a new payment link for a 30-minute English lesson so my student can easily pay me.
    *   As Irina, I want to create a donation link for my fans to support my next album so I can fund my music projects.
    *   As Irina, I want to add a custom image and detailed description to each payment item so it clearly communicates what the payment is for.

### 2.2. Secure Account Authentication (`secure-auth`)

Standard and secure user authentication is foundational for protecting creator data and providing a personalized experience.

*   **Functionality:**
    *   Utilizes Firebase Authentication for robust and secure user management.
    *   **Sign Up:** A user can create an account using an email and password. During sign-up, they will also choose a unique `username` for their public profile URL.
    *   **Log In:** Registered users can log in to access their dashboard.
    *   **Password Recovery:** A 'Forgot Password' flow allows users to securely reset their password via email.

*   **User Stories:**
    *   As Irina, I want to create an account using my email and a secure password so I can start using the platform.
    *   As Irina, I want to reset my password if I forget it so I can regain access to my account.

### 2.3. Personalized Public Profile Page (`profile-customization`)

This feature provides each creator with their own branded corner of the internet, serving as their primary point of contact for their audience.

*   **Functionality:**
    *   Each user gets a unique, public-facing URL based on their chosen username (e.g., `irina.com/irina`).
    *   The page displays information from the `users` collection: `display_name`, `profile_picture_url`, and `bio`.
    *   It includes a section for social media links (e.g., Instagram, YouTube, Twitter) stored in the `social_links` array.
    *   The main content area dynamically fetches and displays all `payment_items` associated with the user that are marked as `is_active`.
    *   The page's theme color (e.g., button colors, link highlights) will be based on the `primaryColor` set in the user's settings, defaulting to the IRINA brand pink (`#ec4899`).

*   **User Stories:**
    *   As Irina, I want to upload a profile picture and write a short bio so my students and fans know who I am.
    *   As Irina, I want to display all my active payment and donation items on my public profile so my audience can easily browse and choose how to support me.

### 2.4. Integrated Payment Processing (`payment-processing`)

This feature ensures seamless and secure handling of all financial transactions.

*   **Functionality:**
    *   Integrates with Stripe Connect Express to onboard creators. During setup, Irina will be redirected to a Stripe-hosted onboarding flow to connect her bank account.
    *   Her `stripe_account_id` is securely stored in her `users` document.
    *   When a fan or student pays on a checkout page (`/pay/[item-id]`), the payment is processed via the Stripe API.
    *   The transaction is created as a direct charge, with funds flowing directly to Irina's connected Stripe account (minus any platform fees).
    *   The checkout page uses Stripe Elements for PCI-compliant, secure credit card input.

*   **User Stories:**
    *   As a student, I want to pay for my English lesson using my credit card directly on the platform without needing an extra account.
    *   As Irina, I want to connect my existing Stripe account so I can receive payments directly into my bank account.

## 3. P1: High-Priority Features (Post-Launch)

### 3.1. Payment & Donation Tracking (`payment-tracking`)

Provides creators with a clear and simple overview of their earnings.

*   **Functionality:**
    *   A 'My Payments' page (`/dashboard/payments`) will display a chronological, filterable list of all successful transactions.
    *   Each entry in the list will show the date, amount, the `title` of the associated `payment_item`, and the `payer_email` (if provided).
    *   The system will listen for successful payment webhooks from Stripe to create a new document in the `transactions` collection, ensuring data reliability.

*   **User Stories:**
    *   As Irina, I want to view a list of all recent payments and donations I've received so I can track my income.
    *   As Irina, I want to see which specific payment item each transaction is for so I can identify if it's for a lesson or a donation.

### 3.2. Account & Payout Settings (`account-settings`)

Allows creators to manage their account details and public-facing information.

*   **Functionality:**
    *   A dedicated 'Settings' page (`/dashboard/settings`) will be divided into sections:
        *   **Profile:** Edit `display_name`, `bio`, `profile_picture_url`, and `social_links`.
        *   **Account:** Change the login email or password.
        *   **Payouts:** View the status of the connected Stripe account and provide a link to the Stripe Express dashboard to manage bank details and view payouts.

*   **User Stories:**
    *   As Irina, I want to update my email address or password for security reasons.
    *   As Irina, I want to update my public profile description or links so my page is always current.
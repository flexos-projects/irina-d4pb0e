---
id: pages
title: 'Pages & Screen Maps'
description: 'An overview of all pages in the IRINA application, outlining their purpose, route, key components, and required authentication state.'
type: doc
subtype: core
status: draft
sequence: 3
tags:
  - ux
  - pages
  - architecture
createdAt: '2023-10-27T10:00:00Z'
updatedAt: '2023-10-27T10:00:00Z'
---

## 1. Introduction

This document provides a comprehensive map of all user-facing pages within the IRINA application. It details the structure and purpose of each screen, distinguishing between publicly accessible pages and those requiring user authentication. This serves as a blueprint for front-end development and user flow design.

## 2. Public Pages (Authentication Not Required)

These pages are accessible to any visitor and are focused on marketing, user conversion, and the core payment experience.

### 2.1. Landing Page
*   **Route:** `/`
*   **Description:** The main marketing page designed to attract and convert new creators. It communicates the value proposition of IRINA.
*   **Sections & Components:**
    *   **Hero Section:** Features the tagline "Your personal hub for sharing creativity and accepting support," a brief, benefit-oriented description, and a prominent 'Create Your Page' call-to-action button.
    *   **Problem/Solution:** A visually engaging section that contrasts the complexity of traditional tools with the simplicity of IRINA.
    *   **Features Overview:** Highlights the core features: Personalized Profile, Simple Payment Items, and Direct Payouts.
    *   **Final CTA:** A concluding section that encourages users to sign up.
    *   **Footer:** Contains links to Terms of Service, Privacy Policy, and social media.

### 2.2. Login / Signup Pages
*   **Route:** `/auth/login`, `/auth/signup`
*   **Description:** Standard forms for user authentication.
*   **Sections & Components:**
    *   **Signup (`/auth/signup`):** Form fields for Email, Password, and a unique Username (which will be validated for availability).
    *   **Login (`/auth/login`):** Form fields for Email and Password, with a 'Forgot Password?' link.
    *   Both pages will feature minimal design to focus the user on the task at hand.

### 2.3. Creator's Public Profile Page
*   **Route:** `/[username]` (e.g., `/irina`)
*   **Description:** The personalized, public-facing hub for a creator. This is the primary page their audience will visit.
*   **Sections & Components:**
    *   **Profile Header:** Displays the creator's `profile_picture`, `display_name`, and a short `bio`.
    *   **Social Links:** A series of icons linking to the creator's configured social media profiles.
    *   **Payment Items Grid:** The main content area, displaying a grid of cards. Each card represents an active `payment_item` and shows its `image`, `title`, `description`, and `amount`. Each card has a 'Pay' or 'Support' button that links to the respective checkout page.

### 2.4. Payment Checkout Page
*   **Route:** `/pay/[item-id]`
*   **Description:** A secure, single-purpose page for a user to complete a payment for a specific item.
*   **Sections & Components:**
    *   **Item Summary:** Displays the details of the selected `payment_item` (image, title, amount) so the payer has clear context.
    *   **Stripe Payment Element:** An embedded, secure form hosted by Stripe for entering credit card information.
    *   **Pay Button:** A button that shows the total amount and triggers the payment processing.

### 2.5. Payment Confirmation Page
*   **Route:** `/pay/success`
*   **Description:** A page shown to the payer immediately after a successful transaction.
*   **Sections & Components:**
    *   **Success Message:** A clear confirmation message, e.g., "Thank you for your support! Your payment was successful."
    *   **Transaction Details:** Displays a transaction reference ID.
    *   **Call to Action:** A button to navigate back to the creator's profile (`/[username]`).

## 3. Authenticated Pages (Login Required)

These pages form the creator's private dashboard for managing their profile, items, and earnings.

### 3.1. Dashboard
*   **Route:** `/dashboard`
*   **Description:** The main landing page for a logged-in creator, providing an at-a-glance overview and navigation.
*   **Sections & Components:**
    *   **Navigation:** A persistent sidebar with links to Dashboard, My Items, Payments, and Settings.
    *   **Earnings Summary:** A widget showing key stats like 'Total Earnings (30 days)' and 'Total Supporters'.
    *   **Recent Payments:** A list of the 5 most recent transactions.
    *   **My Items Overview:** A condensed list of the creator's payment items with quick actions to 'Share', 'Edit', or 'View' the public link.

### 3.2. My Payment Items
*   **Route:** `/dashboard/items`
*   **Description:** A dedicated page for creating and managing all payment and donation items.
*   **Sections & Components:**
    *   **'Create New Item' Button:** A prominent button that navigates to the item creation form.
    *   **Items Table/List:** A comprehensive list of all items created by the user. Each row displays the item's title, amount, status (Active/Inactive), and creation date. Provides actions to 'Edit' or 'Delete' each item.

### 3.3. Create/Edit Item Form
*   **Route:** `/dashboard/items/new`, `/dashboard/items/[item-id]/edit`
*   **Description:** The form used to create or modify a payment item.
*   **Sections & Components:**
    *   A full-page form containing all fields detailed in the `create-payment-items` feature, including title, description, amount, image upload, etc.
    *   'Save & Publish' and 'Save as Draft' action buttons.

### 3.4. My Payments & Donations
*   **Route:** `/dashboard/payments`
*   **Description:** A detailed log of all incoming transactions.
*   **Sections & Components:**
    *   **Date Range Filter:** Allows the creator to filter transactions by a specific time period.
    *   **Transactions Table:** A sortable table listing all payments, with columns for Date, Payer Email, Item Name, and Amount.

### 3.5. Settings
*   **Route:** `/dashboard/settings`
*   **Description:** The central hub for managing all account, profile, and payment-related settings.
*   **Sections & Components:**
    *   **Tabbed Interface:** Organized into 'Profile', 'Account', and 'Payouts'.
    *   **Profile Tab:** Forms to edit public-facing info like display name, bio, and social links.
    *   **Account Tab:** Forms for changing email and password.
    *   **Payouts Tab:** Displays the connection status of the Stripe account and provides a button to 'Manage Stripe Account', which links out to the Stripe Express dashboard.
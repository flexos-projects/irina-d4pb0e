---
id: flows
title: 'User Flows & Journeys'
description: 'Describes the key step-by-step user journeys within the IRINA application, from onboarding to receiving a payment.'
type: doc
subtype: core
status: draft
sequence: 5
tags:
  - ux
  - flows
  - product
createdAt: '2023-10-27T10:00:00Z'
updatedAt: '2023-10-27T10:00:00Z'
---

## 1. Introduction

This document maps out the critical user flows for the IRINA platform. These flows describe the step-by-step journey a user takes to accomplish a key goal. Understanding these paths is essential for creating an intuitive, seamless, and effective user experience. We will cover three primary flows: a creator's initial onboarding, a supporter's payment process, and a creator's ongoing management of their account.

## 2. Flow 1: Creator Onboarding & First Payment Item Creation

*   **Actor:** Irina (a new creator)
*   **Trigger:** Irina discovers IRINA via social media and visits the landing page.
*   **Goal:** To create an account, set up a basic profile, and publish her first payment item for an English lesson.

| Step | Actor  | Action                                                                                                                                                                                          |
| :--- | :----- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Irina  | Visits the landing page (`/`). Reads the value proposition and clicks the 'Create Your Page' button.                                                                                          |
| 2    | Irina  | Is directed to the signup page (`/auth/signup`). She enters her email, a secure password, and chooses a unique username, `irina-k`. She submits the form.                                         |
| 3    | System | Creates a new user account in Firebase Authentication and a corresponding document in the `users` collection with her `uid`, `email`, and `username`. Redirects her to the dashboard (`/dashboard`). |
| 4    | System | Displays a welcome modal or a guided setup tour. The first step prompts her to set her `display_name` ("Irina K.") and upload a `profile_picture`.                                             |
| 5    | Irina  | She provides her name and uploads a photo. She clicks 'Next'.                                                                                                                                   |
| 6    | System | The next step prompts her to connect her Stripe account to receive payments. A 'Connect with Stripe' button is displayed prominently, along with a 'Skip for now' option.                        |
| 7    | Irina  | She clicks 'Connect with Stripe'.                                                                                                                                                               |
| 8    | System | Redirects Irina to the Stripe Connect Express onboarding flow in a new tab.                                                                                                                     |
| 9    | Irina  | She completes the Stripe form, connecting her bank account. Upon completion, Stripe redirects her back to the IRINA settings page.                                                                |
| 10   | System | The system receives the `stripe_account_id` from Stripe and saves it to her `users` document. The UI in her settings now shows 'Stripe Account Connected'.                                       |
| 11   | System | The onboarding guide now directs her to create her first item, navigating her to `/dashboard/items/new`. The form fields might be pre-filled with placeholder examples.                             |
| 12   | Irina  | She fills out the form: Title: "1-hour English Conversation Practice", Amount: 2500 (for $25.00 USD), Description: "A relaxed one-on-one session to improve your spoken English." She uploads an image. |
| 13   | Irina  | She clicks 'Save & Publish'.                                                                                                                                                                    |
| 14   | System | A new document is created in the `payment_items` collection. A success notification appears with her new item's shareable link. She is redirected to her main item list (`/dashboard/items`).         |

**Outcome:** Irina has a fully functional account, a public profile at `/irina-k`, and a live payment item ready to be shared with her students.

## 3. Flow 2: Student/Fan Makes a Payment

*   **Actor:** Alex (a student or fan)
*   **Trigger:** Irina shares the link to her profile or a specific payment item on her Instagram.
*   **Goal:** To pay for an English lesson.

| Step | Actor  | Action                                                                                                                                                                                           |
| :--- | :----- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Alex   | Clicks the link in Irina's Instagram bio, which leads to her public profile (`/irina-k`).                                                                                                        |
| 2    | System | The page loads, displaying Irina's profile picture, bio, and a grid of her active payment items.                                                                                                 |
| 3    | Alex   | He finds the item titled "1-hour English Conversation Practice" and clicks the 'Pay $25.00' button.                                                                                             |
| 4    | System | Navigates Alex to the dedicated checkout page (`/pay/[item-id]`). The page clearly displays the item title, image, and amount to be paid.                                                          |
| 5    | Alex   | Enters his name, email, and credit card details into the secure Stripe Payment Element form on the page.                                                                                         |
| 6    | Alex   | Clicks the 'Confirm Payment' button.                                                                                                                                                            |
| 7    | System | The frontend securely submits the payment details to the Stripe API. Stripe processes the payment, charging Alex's card and associating the funds with Irina's connected Stripe account.             |
| 8    | System | Upon successful processing, Stripe sends a confirmation. The system's backend (via a webhook) creates a new document in the `transactions` collection with all relevant details.                     |
| 9    | System | Alex is redirected to the success page (`/pay/success`), which confirms his payment was completed.                                                                                               |
| 10   | System | An email notification is sent to Irina (future feature) informing her of the new payment. The new transaction immediately appears in her dashboard's 'Recent Payments' list.                     |

**Outcome:** Alex has successfully paid for a lesson, and Irina has received the funds in her Stripe account and a record of the transaction in her IRINA dashboard.

## 4. Flow 3: Creator Manages Profile & Items

*   **Actor:** Irina (an existing creator)
*   **Trigger:** Irina wants to add a new donation item for her music.
*   **Goal:** To add a new item and update her public bio.

| Step | Actor  | Action                                                                                                                                                                                          |
| :--- | :----- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Irina  | Logs into her account and lands on the dashboard (`/dashboard`).                                                                                                                                |
| 2    | Irina  | She wants to add a new item, so she clicks on 'My Items' in the sidebar navigation.                                                                                                             |
| 3    | Irina  | On the `/dashboard/items` page, she clicks the 'Create New Item' button.                                                                                                                        |
| 4    | Irina  | She creates a new donation item: Title: "Support My New Song", Amount: 500 ($5.00), Description: "Help me cover studio costs for my next single! Every bit helps." She sets the type to 'Donation'. |
| 5    | Irina  | She saves the item. It now appears in her list of items and is live on her public profile.                                                                                                      |
| 6    | Irina  | Next, she decides to update her bio. She clicks 'Settings' in the sidebar.                                                                                                                      |
| 7    | Irina  | On the `/dashboard/settings` page, she ensures the 'Profile' tab is selected. She edits the text in the 'Bio' field to mention her new song project.                                             |
| 8    | Irina  | She clicks 'Save Changes'.                                                                                                                                                                      |
| 9    | System | The `bio` field in her `users` document is updated. A success toast notification appears.                                                                                                       |
| 10   | Irina  | She clicks the 'View Public Profile' link to open `/irina-k` in a new tab and confirms that both her new donation item and her updated bio are visible to the public.                             |

**Outcome:** Irina has successfully updated her offerings and her public profile to reflect her current projects.
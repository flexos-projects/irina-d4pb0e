---
id: technical
title: 'Technical Architecture & Stack'
description: 'A detailed overview of the technology stack, architecture, key packages, and API integrations for the IRINA project.'
type: doc
subtype: core
status: draft
sequence: 7
tags:
  - architecture
  - tech-stack
  - firebase
createdAt: '2023-10-27T10:00:00Z'
updatedAt: '2023-10-27T10:00:00Z'
---

## 1. Architecture Overview

The IRINA platform will be built as a modern, serverless web application. This architecture is chosen for its scalability, low operational overhead, and rapid development capabilities. The core components are a Nuxt 4 frontend, which handles all rendering and user interaction, and a Firebase backend, which provides authentication, database, and hosting services. Payments are handled by integrating directly with the Stripe API.

This stack allows for a monolithic repository (`monorepo`) approach where both the frontend and backend functions can be managed together, simplifying development and deployment workflows.

## 2. Technology Stack

### 2.1. Frontend

*   **Framework:** **Nuxt 4**
    *   **Reasoning:** Nuxt provides a powerful, opinionated framework on top of Vue.js. Its file-based routing, server-side rendering (SSR) capabilities for SEO on public profile pages, and rich module ecosystem make it ideal for building a performant and maintainable application. The upcoming Nuxt 4 release ensures we are building on the latest technology.

*   **Styling:** **Tailwind CSS**
    *   **Reasoning:** A utility-first CSS framework that allows for rapid UI development without writing custom CSS. It's highly configurable, enabling us to easily implement the design system's color palette and typography.

*   **State Management:** **Pinia**
    *   **Reasoning:** The official state management library for Vue. It's lightweight, intuitive, and integrates perfectly with the Vue 3 Composition API used in Nuxt, making it easy to manage global state like the current user's authentication status and profile data.

### 2.2. Backend & Infrastructure

*   **Database:** **Firebase Firestore**
    *   **Reasoning:** A scalable, real-time NoSQL document database. Its flexible data model is perfect for our schema (`users`, `payment_items`, `transactions`), and its real-time capabilities can provide live updates in the dashboard. The robust security rules engine is critical for protecting user data.

*   **Authentication:** **Firebase Authentication**
    *   **Reasoning:** A secure, managed authentication service that handles user sign-up, login, and password recovery out of the box. It integrates seamlessly with Firestore security rules, simplifying access control.

*   **Hosting:** **Firebase Hosting**
    *   **Reasoning:** Provides fast, secure, and reliable hosting with a global CDN. Its integration with Firebase Functions allows us to easily deploy our Nuxt SSR application and any backend functions in a single environment.

*   **Backend Logic:** **Firebase Cloud Functions**
    *   **Reasoning:** For any secure backend logic, such as handling Stripe webhooks or performing complex writes, Cloud Functions provide a serverless environment. This is essential for creating transaction records securely after a payment is confirmed by Stripe.

### 2.3. Key Packages & Libraries

*   **`@nuxtjs/supabase` (or a similar Firebase module):** To provide simple composables (`useCurrentUser`, `useFirestore`) for interacting with the Firebase backend from within the Nuxt application.
*   **`@stripe/stripe-js` & `@stripe/vue-stripe-js`:** The official Stripe libraries for securely embedding the Stripe Payment Element in the checkout page to handle credit card input without sensitive data ever touching our servers.
*   **`VueUse`:** A collection of essential Vue Composition API utilities that will accelerate development by providing ready-to-use functions for common tasks.

## 3. API Integrations

### 3.1. Stripe API

Stripe is the cornerstone of our payment processing. We will use two key products:

*   **Stripe Connect Express:** This is the primary integration. When a creator (e.g., Irina) signs up for payments, we will use the Connect API to create an Express account for her. She will be redirected to a Stripe-hosted onboarding flow to provide her identity and bank details. We will store the resulting `stripe_account_id` in her `users` document. This model allows us to facilitate payments without taking on the liability of holding funds.

*   **Stripe Payments (with Payment Intents API):** When a customer pays, our backend will create a `PaymentIntent` on behalf of the connected creator. The flow is as follows:
    1.  User clicks 'Pay' on the frontend.
    2.  The frontend requests a `client_secret` from a secure backend endpoint.
    3.  The backend creates a Stripe `PaymentIntent`, specifying the amount, currency, and the creator's `stripe_account_id` as the destination for the funds.
    4.  The backend returns the `client_secret` to the frontend.
    5.  The frontend uses the `client_secret` with Stripe.js to confirm the payment via the embedded Payment Element.

*   **Stripe Webhooks:** We will configure a webhook endpoint using a Firebase Cloud Function. This endpoint will listen for the `charge.succeeded` event from Stripe. When this event is received, the function will securely create a new document in the `transactions` collection, ensuring a reliable record of all successful payments.

## 4. Deployment & DevOps

*   **Repository:** A single monorepo hosted on GitHub will contain the Nuxt application and Firebase Functions code.
*   **CI/CD:** GitHub Actions will be used to automate testing and deployment. A workflow will be configured to deploy the Nuxt application and all Cloud Functions to Firebase Hosting upon a push to the `main` branch.
*   **Environments:** We will maintain at least two Firebase projects: one for development (`irina-dev`) and one for production (`irina-prod`), each with its own separate database, auth instance, and Stripe API keys (Test and Live).
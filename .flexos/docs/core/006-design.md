---
id: design
title: 'Design System & Branding'
description: 'Establishes the visual identity, branding guidelines, and core design principles for the IRINA project.'
type: doc
subtype: core
status: draft
sequence: 6
tags:
  - design
  - branding
  - ui
createdAt: '2023-10-27T10:00:00Z'
updatedAt: '2023-10-27T10:00:00Z'
---

## 1. Design Philosophy & Vibe

The visual identity of IRINA is designed to be **Playful, Authentic, and Supportive**. It should feel energetic and modern, yet personal and approachable, avoiding the sterile, corporate aesthetic of many financial tools. The design must empower creators to express their own brand while maintaining a cohesive and intuitive user experience across the platform.

Our persona, Irina, is an indie musician and teacher with a love for pink and purple. The design system reflects this by using a vibrant, creative color palette and clean, modern typography. The overall vibe should feel like a creative's digital studio—organized, inspiring, and uniquely their own.

**Core Principles:**
*   **Clarity First:** The interface must be clean and intuitive. The primary goal is to make creating items and getting paid as simple as possible. White space is our friend.
*   **Expressive & Personal:** While the core UI is consistent, the creator's public profile should feel like *their* space. The use of their profile picture, custom imagery for items, and a user-selected primary color is key.
*   **Mobile-First:** A significant portion of traffic will come from social media links clicked on mobile devices. All pages, especially the public profile and checkout flow, must be flawlessly responsive and optimized for smaller screens.
*   **Trustworthy & Secure:** As a platform handling payments, the design must convey security and professionalism. This is achieved through clean layouts, clear labeling, and the use of trusted elements like the Stripe Payment Element.

## 2. Color Palette

The color palette is energetic and modern, balancing a vibrant primary color with a versatile accent and a range of functional neutrals.

*   **Primary (`primaryColor`):** `#ec4899` (Pink 500)
    *   **Usage:** The main brand color. Used for primary buttons, active links, key calls-to-action, and as the default theme color for creator profiles. It's bold, confident, and creative.

*   **Accent (`accentColor`):** `#8b5cf6` (Violet 500)
    *   **Usage:** Used for secondary actions, highlights, illustrations, and decorative elements. It complements the primary pink, adding depth and a touch of indie-rock flair.

*   **Neutral Dark (`neutralDark`):** `#1a202c` (Gray 800)
    *   **Usage:** The primary color for all body text, headlines, and dark UI elements. It provides strong, comfortable contrast against light backgrounds.

*   **Neutral Light (`neutralLight`):** `#f8fafc` (Slate 50)
    *   **Usage:** The primary background color for the application, creating a clean, bright, and airy feel.

*   **Supporting Neutrals:**
    *   `#e2e8f0` (Slate 200): For borders, dividers, and disabled states.
    *   `#94a3b8` (Slate 400): For secondary text, labels, and icons.

*   **System Colors:**
    *   **Success:** `#22c55e` (Green 500)
    *   **Warning:** `#f59e0b` (Amber 500)
    *   **Error:** `#ef4444` (Red 500)

## 3. Typography

Typography will be handled using a clean, modern, and highly-readable sans-serif font family.

*   **Font Family:** `Poppins`
    *   **Reasoning:** Poppins is a geometric sans-serif with a friendly and approachable feel. Its clean letterforms ensure excellent legibility at various sizes and weights, making it suitable for both headlines and body copy.

*   **Hierarchy:**
    *   **h1 / Page Titles:** Poppins Bold, 36px
    *   **h2 / Section Titles:** Poppins Bold, 24px
    *   **h3 / Sub-headings:** Poppins Semibold, 20px
    *   **Body Text:** Poppins Regular, 16px
    *   **Labels / Small Text:** Poppins Medium, 14px

## 4. UI Components & Patterns

*   **Buttons:**
    *   **Primary:** Solid background (`primaryColor`), white text. Used for the most important actions (e.g., 'Save & Publish', 'Pay Now').
    *   **Secondary:** Outlined with `primaryColor` text and border, transparent background. Used for less critical actions (e.g., 'View Profile', 'Cancel').
    *   **State:** Clear hover, active, and disabled states to provide user feedback.

*   **Forms:**
    *   Inputs will have a clean, minimalist design with a light gray border that becomes `primaryColor` on focus.
    *   Labels will be placed above the input fields for clarity.
    *   Validation messages will appear below the field in the 'Error' color.

*   **Cards:**
    *   Used extensively for displaying `payment_items` on the public profile and dashboard.
    *   Cards will have a subtle border-radius, a light box-shadow on hover to indicate interactivity, and a clear visual hierarchy for the image, title, description, and price.

*   **Navigation:**
    *   **Public Site:** A simple header with the IRINA logo and 'Login'/'Sign Up' buttons.
    *   **Authenticated Dashboard:** A persistent vertical sidebar on the left for primary navigation (Dashboard, Items, Payments, Settings), allowing the main content area to be the focus. The active link will be highlighted using `primaryColor`.

## 5. Logo & Branding

The IRINA logo should be a simple, text-based logotype using the Poppins font, perhaps with a custom flourish or a subtle icon that represents connection or creation. The primary color (`#ec4899`) should be central to the logo's identity. The branding should feel personal and creator-first, as if it were a tool designed by a creator, for a creator.
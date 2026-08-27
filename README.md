<div align="center">

# Faylish

### Full-Stack E-Commerce Platform

A comprehensive, full-stack e-commerce application built for modern selling — supporting Admins, Sellers, and Customers in a single unified platform.

[![Made with Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white)](#technology-stack)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](#technology-stack)
[![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](#technology-stack)
[![Clerk Auth](https://img.shields.io/badge/Clerk-6C47FF?style=for-the-badge&logo=clerk&logoColor=white)](#technology-stack)

</div>

---

## Table of Contents

- [Overview](#overview)
- [Features Overview](#features-overview)
- [Technology Stack](#technology-stack)
- [System Architecture](#system-architecture)
- [Project Folder Structure](#project-folder-structure)
- [Core Modules](#core-modules)
- [How AI-Powered Product Listing Works](#how-ai-powered-product-listing-works)
- [Setup & Installation](#setup--installation)
- [Environment Variables](#environment-variables)
- [Running the Project](#running-the-project)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Contact & Support](#contact--support)

---

## Overview

Faylish is a comprehensive, full-stack e-commerce application built for modern selling. Whether you're an admin managing the platform, a seller running your own store, or a customer shopping for the best products, Faylish provides a seamless, scalable, and feature-rich experience.

This documentation provides a detailed breakdown of the project's structure, technologies, features, and setup instructions.

### Project Goals

- Provide a multi-tenant marketplace where independent sellers can run their own stores
- Automate and simplify product listing using AI-powered image analysis
- Offer a smooth, modern shopping experience with real-time cart and secure checkout
- Give admins full control over store approvals, coupons, and platform health

---

## Features Overview

### Customer Features

| Feature | Description |
|---|---|
| Authentication | Seamless sign-in via Clerk (Google, Email, and more) |
| Shop & Product Search | Advanced search with category-based filtering |
| Cart System | Live quantity updates and price calculations via Redux Toolkit |
| Secure Checkout | Stripe-powered checkout with coupon support |
| Order History | View past orders and track status |
| Ratings & Reviews | Rate and review purchased products |
| Address Management | Save and manage multiple shipping addresses |

### Seller Features

| Feature | Description |
|---|---|
| Store Creation | Application form with logo and store info upload |
| AI-Powered Product Listing | Upload a product image and the AI auto-generates a name and description |
| Product Management | Add, view, and toggle stock status for products |
| Order Management | View customer details, order items, and update order status (Placed, Processing, Shipped, Delivered) |
| Seller Dashboard | Track earnings, total orders, products, and customer reviews |

### Admin Features

| Feature | Description |
|---|---|
| Platform Dashboard | View total revenue, products, orders, and live stores |
| Store Management | Approve or reject new seller applications |
| Coupon Management | Create, edit, and delete discount coupons |
| Store Activation | Toggle store activity on or off |

---

## Technology Stack

| Category | Technology |
|---|---|
| Frontend | Next.js (App Router), React, Tailwind CSS, Framer Motion, Lucide Icons |
| State Management | Redux Toolkit (custom store configuration) |
| Backend | Next.js API Routes (serverless functions) |
| Database | PostgreSQL (Neon Serverless via Prisma) |
| Authentication | Clerk |
| Payments | Stripe |
| AI Integration | OpenAI API (GPT-4o with Vision) |
| File Uploads | ImageKit (CDN for optimized images) |
| Notifications | react-hot-toast |

---

## System Architecture

```
+-----------------------------------------------------------------+
|                          CLIENT LAYER                            |
+-----------------------------------------------------------------+
|   Public Storefront   |   Seller Dashboard   |   Admin Panel     |
|   app/(public)        |   app/store          |   app/admin       |
+-----------+----------------------+-------------------+-----------+
            |                      |                   |
+-----------v----------------------v-------------------v-----------+
|                     NEXT.JS API ROUTES (app/api)                 |
+--------------------------------------------------------------------+
| Address | Cart | Coupon | Orders | Products | Rating | Stripe      |
| Admin/* | Store/* (ai, create, dashboard, orders, product)         |
+-----------+----------------------------------------------+---------+
            |                                              |
+-----------v-----------+                       +----------v----------+
|   External Services    |                       |   PostgreSQL (Neon)  |
+-------------------------+                       |     + Prisma ORM     |
| Clerk (Auth)            |                       +-----------------------+
| Stripe (Payments)       |
| OpenAI (AI listings)    |
| ImageKit (Media CDN)    |
+-------------------------+
```

---

## Project Folder Structure

### 1. Root Directory

```
├── assets/                 # Static assets (dummy data, images)
├── components/             # Reusable React components (UI)
├── configs/                # External service configurations (ImageKit, OpenAI)
├── inngest/                # Background jobs/events (if used)
├── lib/                    # Prisma client and Redux store
├── middlewares/            # Custom authentication middlewares (authSeller)
├── prisma/                 # Database schema and migrations
├── public/                 # Public static files
├── middleware.js           # Next.js global middleware
├── next.config.js          # Next.js configuration
├── package.json            # Project dependencies
└── postcss.config.js       # Tailwind CSS configuration
```

### 2. The `app/` Directory (Next.js App Router)

The application uses a route group structure to separate concerns.

**a) `app/(public)` — Public Facing Pages**

```
(public)/
├── about/page.jsx          # Company about page
├── cart/page.jsx           # Shopping cart page
├── contact/page.jsx        # Contact form and info
├── create-store/page.jsx   # Seller application form
├── loading/page.jsx        # Custom loading page
├── orders/page.jsx         # User order history
├── pricing/page.jsx        # Pricing information (if applicable)
├── product/page.jsx        # Product details view
├── shop/page.jsx           # Main product listing/shop
├── layout.jsx              # Public layout (Navbar, Footer, Redux providers)
└── page.jsx                # Home landing page
```

**b) `app/store` — Seller Dashboard Pages**

```
store/
├── dashboard/page.jsx       # Seller metrics (Earnings, Orders, Ratings)
├── add-product/page.jsx     # Add new products (with AI integration)
├── manage-products/page.jsx # View, toggle stock for existing products
├── orders/page.jsx          # Manage incoming orders and statuses
├── layout.jsx               # Store layout (Auth checks + StoreLayout)
└── page.jsx                 # Store main redirect (login gate)
```

**c) `app/admin` — Administrator Dashboard Pages**

```
admin/
├── approve/page.jsx         # Approve/Reject pending seller stores
├── coupons/page.jsx         # Add/Delete platform coupons
├── stores/page.jsx          # Activate/Deactivate live stores
├── layout.jsx               # Admin layout (Auth checks + AdminLayout)
└── page.jsx                 # Admin main dashboard
```

**d) `app/api` — Backend API Routes**

```
api/
├── address/                 # CRUD for user shipping addresses
├── admin/
│   ├── coupon/route.js      # Admin coupon management
│   ├── dashboard/route.js   # Admin dashboard stats
│   ├── approve-store/route.js # Approve/Reject stores
│   ├── stores/route.js      # List all stores
│   └── toggle-store/route.js # Toggle store activation status
├── cart/                    # Sync user cart with database
├── coupon/                  # Validate coupons
├── inngest/                 # Webhook for background processes
├── orders/                  # Create and fetch user orders
├── products/                # Fetch product listings
├── rating/                  # Submit and fetch product reviews
├── Store/                   # Seller-specific endpoints
│   ├── ai/route.js          # AI image analysis for product details
│   ├── create/route.js      # Create/check store status
│   ├── dashboard/route.js   # Seller dashboard metrics
│   ├── orders/route.js      # Manage seller orders
│   ├── product/route.js     # Add/Get seller products
│   └── stock-toggle/route.js# Toggle product stock
└── stripe/route.js          # Stripe webhook for payment verification
```

### 3. The `components/` Directory

Contains reusable UI components organized by context:

```
components/
├── admin/                   # Components for Admin Panel (e.g., StoreInfo)
├── store/                   # Components for Seller Dashboard (e.g., StoreLayout)
├── AddressModal.jsx         # Modal for managing addresses
├── Banner.jsx               # Promotional top bar
├── BestSelling.jsx          # Best selling products carousel
├── Carousel.jsx             # Main Hero carousel
├── CategoriesMarquee.jsx    # Scrolling text of categories
├── Counter.jsx              # Quantity selector component
├── Footer.jsx               # Global footer
├── Hero.jsx                 # Main Hero section
├── Hero2.jsx                # Secondary Hero section
├── LatestProducts.jsx       # Latest products grid
├── Loading.jsx              # Global loading spinner
├── Navbar.jsx               # Global navigation header
├── Newsletter.jsx           # Newsletter signup section
├── OrderItem.jsx            # Individual order item component
├── OrdersAreaChart.jsx      # Chart for admin dashboard
├── OrderSummary.jsx         # Cart/Checkout total summary
├── OurSpec.jsx              # Brand value proposition specs
└── PageTitle.jsx            # Reusable page header title
```

### 4. Supporting Directories

| Directory | Purpose |
|---|---|
| `configs/` | Centralizes initialization of external services like `imagekit.js` (image uploads) and `openai.js` (AI APIs) |
| `lib/` | Contains `store.js` (Redux configuration) and `prisma.js` (database client setup) |
| `middlewares/` | Contains `authSeller.js`, which verifies that a user is an active seller before allowing access to specific API routes |
| `prisma/` | Holds `schema.prisma`, describing the database tables (User, Store, Product, Order, Rating, etc.) |

---

## Core Modules

### Authentication
- Managed entirely through Clerk (Google OAuth, email/password, and more)
- Role distinction handled at the database/middleware level (Customer, Seller, Admin)

### Store Lifecycle
- A user applies to become a seller via `create-store`
- Admin reviews the application in `admin/approve`
- Once approved, the store becomes active and products can be listed
- Admins retain the ability to deactivate a store at any time via `admin/stores`

### Product Management
- Sellers add products with images, pricing, category, and stock
- AI can auto-generate the name and description from the uploaded image
- Stock status can be toggled on or off without deleting the listing

### Cart & Checkout
- Cart state is synced between Redux (client-side) and the database
- Coupons are validated server-side before being applied
- Checkout is handled through Stripe, with webhook-based payment verification

### Order Management
- Orders move through a defined status flow: `Placed -> Processing -> Shipped -> Delivered`
- Sellers view and update the status of their own orders
- Customers can view full order history and leave ratings on delivered items

---

## How AI-Powered Product Listing Works

When a seller uploads an image on the "Add Product" page:

1. The image is converted to a Base64 string on the client.
2. It is sent to the `/api/Store/ai` API route.
3. The route authenticates the seller using the `authSeller` middleware.
4. The image and a prompt are sent to OpenAI (GPT-4o with Vision).
5. OpenAI analyzes the image (for example, a red lipstick) and returns raw JSON containing a suggested product name and description.
6. The frontend automatically fills in the corresponding form fields, which the seller can review and edit before submitting.

---

## Setup & Installation

### Prerequisites

- Node.js (v18 or higher)
- PostgreSQL database (Neon recommended)
- Clerk account
- Stripe account
- ImageKit account
- OpenAI API key

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/faylish.git
cd faylish
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file in the root directory (see [Environment Variables](#environment-variables) below).

### 4. Set Up the Database

```bash
npx prisma generate
npx prisma migrate dev
```

---

## Environment Variables

Create a `.env` file in the root directory with the following keys:

```bash
# Database
DATABASE_URL="your_postgresql_connection_string"

# Authentication (Clerk)
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="your_clerk_publishable_key"
CLERK_SECRET_KEY="your_clerk_secret_key"

# Payments (Stripe)
STRIPE_SECRET_KEY="your_stripe_secret_key"
STRIPE_WEBHOOK_SECRET="your_stripe_webhook_secret"
NEXT_PUBLIC_CURRENCY_SYMBOL="$"

# File Uploads (ImageKit)
IMAGEKIT_PUBLIC_KEY="your_imagekit_public_key"
IMAGEKIT_PRIVATE_KEY="your_imagekit_private_key"
IMAGEKIT_URL_ENDPOINT="your_imagekit_url_endpoint"

# AI Integration (OpenAI)
OPENAI_API_KEY="your_openai_api_key"
OPENAI_BASE_URL="your_openai_base_url"
OPENAI_MODEL="gpt-4o"
```

> Never commit your `.env` file. Make sure it is listed in `.gitignore` before pushing to a public repository.

---

## Running the Project

```bash
npm run dev
```

Navigate to `http://localhost:3000` to view the application.

---

## Roadmap

- [ ] Mobile-optimized PWA support
- [ ] Multi-currency support
- [ ] Real-time order notifications
- [ ] Advanced seller analytics
- [ ] Wishlist functionality
- [ ] Multi-language storefront
- [ ] Bulk product import for sellers
- [ ] Refund and return management

---

## Contributing

Contributions are welcome. To contribute:

1. Fork the repository
2. Create your feature branch
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. Commit your changes
   ```bash
   git commit -m 'Add some amazing feature'
   ```
4. Push to the branch
   ```bash
   git push origin feature/amazing-feature
   ```
5. Open a Pull Request

---

## Contact & Support

- **Project Repository:** GitHub
- **Issues:** GitHub Issues

<div align="center">

Built by Anosha

</div>

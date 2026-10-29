# Allure

A full-stack e-commerce web application built with Vue 3 that provides a seamless shopping experience for customers and product management tools for administrators.

**[Live Demo](https://allure-neon.vercel.app)**

## Demo Accounts

Explore the application using the following test accounts.

| Role | Email | Password |
|---|---|---|
| Admin | `admin@mail.com` | `admin1234` |
| Customer | `maxfury@mail.com` | `maxfuryforlife` |

## Features

### Customer Experience
- **Authentication** — Register and log in using JWT-based authentication.
- **Product Catalog** — Browse active products and view individual product details.
- **Shopping Cart** — Add products, update quantities, remove items, and view subtotals and the cart total.
- **Checkout** — Place orders and view order confirmations.
- **Profile Management** — View and update profile information.

### Admin Dashboard
- **Product Management** — Create, edit, deactivate, and reactivate products.
- **Role-Based Access** — Access administrative functionality through protected routes.

### Additional Features
- Dynamic navigation based on authentication status.
- Light and dark theme toggle.
- Toast notifications using Notyf.
- Client-side routing with Vue Router.
- Centralized application state using Pinia.
- Axios API integration with automatic JWT authorization headers.

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend Framework | Vue 3 |
| Build Tool | Vite |
| State Management | Pinia |
| Routing | Vue Router |
| HTTP Client | Axios |
| Styling | Bootstrap 5, Bootstrap Icons |
| Notifications | Notyf |
| Authentication | JWT-based authentication |
| Deployment | Vercel |

## Getting Started

### Prerequisites
- Node.js 18 or higher
- npm
- Access to a running backend API

### Installation

```bash
git clone https://github.com/deyperfect/allure.git
cd allure
npm install
```

### Environment Configuration

Create a `.env.local` file in the project root:

```env
VITE_JOB_TRACKER_API=http://localhost:4000
```

Set the variable to your deployed backend API URL when connecting to the production backend.

**Important:** Vite exposes variables prefixed with `VITE_` to client-side code. Only use this variable for public configuration, such as an API base URL. Never store passwords, private API keys, or other secrets in it.

### Run Locally

Start the development server:

```bash
npm run dev
```

Open the local URL shown in your terminal, usually `http://localhost:5173`.

### Build for Production

```bash
npm run build
```

### Preview the Production Build

```bash
npm run preview
```

## Project Structure

```text
src/
├── api.js
├── main.js
├── App.vue
├── stores/
│   └── global.js
├── components/
│   ├── NavbarComponent.vue
│   ├── ThemeToggle.vue
│   ├── EditProductButton.vue
│   └── ToggleProductStatusButton.vue
└── pages/
    ├── RegisterPage.vue
    ├── LoginPage.vue
    ├── ProductsPage.vue
    ├── ProductDetailsPage.vue
    ├── CartViewPage.vue
    ├── CheckoutPage.vue
    ├── OrderConfirmationPage.vue
    ├── AdminDashboardPage.vue
    ├── AddProductPage.vue
    ├── EditProductPage.vue
    └── ProfilePage.vue
```

## Deployment

The frontend is deployed on [Vercel](https://vercel.com/).

To deploy your own instance:

1. Import the GitHub repository into Vercel.
2. Configure the build settings for your Vite application.
3. Set `VITE_JOB_TRACKER_API` in the Vercel project environment variables to your backend API URL.
4. Ensure the backend permits requests from your deployed frontend through its CORS configuration.
5. Deploy the application and verify the customer and admin workflows.

Subsequent deployments can run automatically when you push changes to the configured production branch.

## Authors

- **Edryl Palinis**
- **Norman Chingcuanco**

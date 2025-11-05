# Italian E-Commerce Checkout Application

## Overview

This is a full-stack e-commerce checkout application tailored for the Italian market, providing a streamlined checkout experience akin to Shopify and WooCommerce. It features a React frontend with shadcn/ui and an Express backend with PostgreSQL (via Drizzle ORM). The application manages the entire order lifecycle from submission to confirmation, supporting Italian-specific shipping and payment methods. It also includes a robust CMS for managing static content pages and an admin panel for order and product management, pixel tracking, and SMS notifications. The project aims to deliver a modern, localized e-commerce solution with advanced features for conversion tracking and customer communication.

## User Preferences

Preferred communication style: Simple, everyday language.

## Security Notes

- **Admin Credentials**: Admin access is secured via environment variables (`ADMIN_USERNAME` and `ADMIN_PASSWORD`) stored in Replit Secrets. Credentials are never hardcoded in the application files.
- **Password Hashing**: All admin passwords are hashed using scrypt with random salts before storage.
- **Session Security**: Session management requires a `SESSION_SECRET` environment variable. The application will not start without this variable set, preventing the use of insecure default secrets. Session cookies are httpOnly and secure in production.
- **Rate Limiting**: Login attempts are rate-limited to 5 attempts per 15 minutes per IP address to prevent brute force attacks. Failed attempts are logged with IP addresses and timestamps for security monitoring.
- **No Registration Endpoint**: The admin registration endpoint has been completely removed. Admin users cannot be created via API, preventing unauthorized account creation.
- **Security Logging**: All login attempts (successful and failed) are logged with IP addresses, usernames, and timestamps for security auditing and monitoring.
- **Search Engine Protection**: A `robots.txt` file blocks search engines from indexing the `/idara` admin paths, reducing attack surface visibility.

## System Architecture

### UI/UX Decisions
The frontend is built with React 18, TypeScript, and Vite, utilizing `shadcn/ui` components based on Radix UI for a consistent design system. Styling is managed with Tailwind CSS, incorporating a custom color system, responsive design (mobile-first), and the Inter font family. The design follows established e-commerce patterns with a two-column desktop layout (form and order summary) and a single-column mobile layout, emphasizing trust-building elements.

### Technical Implementations
- **Frontend**: State management uses React Query for server state and React Hook Form with Zod for form handling. `wouter` handles client-side routing.
- **Home Page**: A responsive product catalog page (`/`) displays all available products in a grid layout (1 column mobile, 2-3 tablet, 3-4 desktop). Features include product cards with images, ratings, pricing, discount badges, and clickable links to individual product pages. The page uses the same header and footer design as product pages, includes SEO meta tags (title, description, Open Graph), and follows mobile-first responsive design principles.
- **Backend**: Express.js with TypeScript in ESM mode provides a RESTful API. A storage abstraction (`IStorage`) allows swapping between in-memory and PostgreSQL (Drizzle ORM) data layers. Custom middleware handles logging and error handling, with Zod for request validation.
- **Admin Panel**: Features include a protected admin interface (`/idara`) with session-based authentication (Passport.js, scrypt hashing), CRUD operations for products and CMS pages, order management (including bulk tracking number updates via CSV, CSV export, status tracking, and courier field management), order search functionality filtering by order ID, customer name, phone number, and city, SMS settings configuration, dark/light theme support, and an analytics dashboard with date filtering (All Time, Today, Yesterday, Last 3 Days, This Week, Custom Range) for viewing order statistics and revenue metrics across different time periods.
- **Pixel Tracking System**: A centralized pixel manager supports Facebook and TikTok conversion tracking. Each product can have unique pixel and CAPI access tokens. Server-side Facebook Conversion API (CAPI) events are sent for purchases, complementing browser-side tracking for `ViewContent`, `InitiateCheckout`, and `Purchase`. User data is SHA-256 hashed for CAPI events.
- **SMS Notifications**: Integration with Bulkgate allows automatic SMS notifications for order creation and completion. Phone numbers are normalized to E.164 format for Italian numbers. SMS templates are customizable via the admin panel with support for variables like `{name}`, `{orderId}`, `{total}`.
- **Content Management System (CMS)**: An admin interface at `/idara/pages` provides full CRUD for static content pages. Pages are accessible at `/:slug`, with control over published status.
- **Footer Management**: A structured footer editor at `/idara/pages` (Footer tab) allows editing individual footer components through dedicated form fields: logo text, subtitle/help text, WhatsApp widget (phone number and button text), email widget (address and label text), dynamic footer links (title/URL pairs), and copyright notice. The footer is stored as structured data rather than raw HTML, providing type-safe editing and rendering. The footer uses a grey background (#f5f5f5) with black text and icons, and is displayed consistently across all pages including product pages and the thank you page.
- **Header Logo Management**: Admins can toggle between a text logo (displaying the logo text from footer settings in italic font) and an image logo via the footer settings panel. When "Image Logo" is selected, admins can provide a custom logo image URL which will be displayed on the product page header. If no custom URL is provided, the system falls back to a default orange "VS" icon with "Vigoshop" text. The admin panel includes a live preview of the custom logo image, allowing admins to verify the logo appearance before saving. This provides flexible branding control for the product page header.
- **Order Management Enhancements**: Orders table includes a search box for real-time filtering, date & time column showing when each order was placed (displayed in GMT+1/Italy timezone), courier field with inline editing capability, and enhanced CSV bulk import supporting 3 columns (Order ID, Tracking Number, and optional Courier). CSV exports include the order date and time.
- **SEO Management**: Comprehensive SEO settings at `/idara/seo` allow admins to configure global SEO metadata including website title, meta description, favicon URL, keywords, viewport, charset, Open Graph tags (title, description, image, type, URL), theme color, and manifest URL. Product pages use product-specific SEO fields (metaDescription, metaKeywords) when available, falling back to global settings. All pages dynamically apply SEO metadata via the `useSeo` hook.
- **Product Image Management**: Admin panel supports uploading product images via Replit's App Storage. Each product can have multiple images with individual alt text for accessibility and SEO. The image gallery in the admin form allows reordering and deleting images. Product images support both legacy URL strings and new {url, alt} objects for backward compatibility.
- **Google Shopping Integration**: Products include Google Shopping fields (brand, GTIN, MPN, SKU, condition, availability) editable in the admin panel. Product pages include JSON-LD Product Schema markup for Google Shopping and rich search results, providing structured data with pricing, ratings, availability, and product details.

### System Design Choices
- **Data Storage**: Designed for PostgreSQL with Drizzle ORM for schema management (`drizzle-kit`), currently using an in-memory `MemStorage` for development, which seeds demo data on startup.
- **Authentication**: Session-based authentication for admin routes using Passport.js. Public checkout remains anonymous.
- **Error Handling**: Centralized error handling with structured JSON responses and Zod schema validation for all incoming requests.
- **Development Environment**: Custom Vite integration for HMR and seamless frontend/backend development.
- **Localization**: Tailored for the Italian market with specific shipping providers, postal code validation, and localized content.

## External Dependencies

### UI & Component Libraries
- **@radix-ui/***: Headless UI primitives
- **shadcn/ui**: Pre-built accessible components
- **Tailwind CSS**: Utility-first CSS framework
- **class-variance-authority**: Type-safe component variants
- **Lucide React**: Icon library

### Data & Forms
- **React Hook Form**: Form state management
- **Zod**: Schema validation
- **@hookform/resolvers**: Zod resolver for React Hook Form
- **drizzle-zod**: Zod schema generation from Drizzle schemas

### Database & ORM
- **Drizzle ORM**: TypeScript ORM for PostgreSQL
- **@neondatabase/serverless**: Neon serverless PostgreSQL driver
- **drizzle-kit**: Migration and schema management

### State Management & Data Fetching
- **@tanstack/react-query**: Server state management

### Routing & Navigation
- **wouter**: Minimalist routing library

### Build Tools & Development
- **Vite**: Frontend build tool and dev server
- **esbuild**: Fast JavaScript bundler
- **TypeScript**: Type safety
- **tsx**: TypeScript execution for development
- **@replit/vite-plugin-***: Replit-specific enhancements

### Utilities
- **clsx**: Conditional className utility
- **tailwind-merge**: Tailwind class merging utility
- **nanoid**: Compact ID generator
- **date-fns**: Date utility library
- **papaparse**: CSV parsing (for bulk tracking updates)
- **express-rate-limit**: Rate limiting middleware for brute force protection

### Third-Party Integrations
- **Bulkgate**: SMS notification service
- **Facebook Conversion API**: Server-side pixel tracking
- **TikTok Pixel**: Browser-side pixel tracking
- **Replit App Storage**: Object storage for uploaded product images

### File Upload & Object Storage
- **@uppy/core**: File upload framework (v4)
- **@uppy/aws-s3**: S3-compatible upload plugin for direct-to-storage uploads
- **@uppy/dashboard**: Modal UI for file selection and upload
- **@uppy/react**: React bindings for Uppy
- **@google-cloud/storage**: Google Cloud Storage client (Replit App Storage backend)
- **google-auth-library**: Authentication for Google Cloud services

### Assets
- Shipping provider logos (BRT, Poste Italiane) referenced via Vite alias `@assets`
- Product images uploaded to Replit App Storage via admin panel
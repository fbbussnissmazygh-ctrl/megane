# Design Guidelines: Italian E-Commerce Checkout Page

## Design Approach

**Reference-Based Approach**: This checkout page follows established e-commerce patterns similar to Shopify, WooCommerce, and modern Italian e-commerce platforms. The design prioritizes clarity, trust, and conversion optimization with a clean, minimal aesthetic focused on completing the purchase flow.

## Core Design Elements

### Typography Hierarchy

**Font Family**: Use a clean, readable sans-serif from Google Fonts
- Primary: Inter or similar modern sans-serif for all UI elements
- Weights: 400 (regular), 500 (medium), 600 (semibold), 700 (bold)

**Hierarchy**:
- Page Title: Not prominent - checkout pages prioritize function over branding
- Section Headers: text-base font-semibold (Shipping Address, Payment Method)
- Form Labels: text-sm font-medium, positioned above inputs
- Product Names: text-sm font-medium
- Prices: text-base font-semibold for totals, text-sm for line items
- Helper Text: text-xs for trust badges and secondary information
- CTA Button: text-base font-semibold uppercase

### Layout System

**Spacing Primitives**: Use Tailwind units of 2, 3, 4, 6, 8, and 12
- Form field spacing: mb-4 between fields
- Section spacing: mb-8 between major sections
- Container padding: p-4 on mobile, p-6 on desktop
- Inner component padding: p-3 to p-4

**Grid Structure**:
- Single column layout on mobile (< 768px)
- Two-column on desktop: Left column (form, 60% width) + Right column (order summary, 40% width)
- Max container width: max-w-6xl centered
- Gap between columns: gap-8

### Component Library

**Form Components**:

Input Fields:
- Full width with border and rounded corners (rounded-md)
- Padding: px-3 py-2.5
- Border width: border (1px)
- Focus state: visible focus ring with offset
- Label positioning: above input with mb-2
- Placeholder text for guidance
- Required asterisk (*) for mandatory fields

Select Dropdowns:
- Identical styling to text inputs
- Dropdown arrow on the right
- Province/City selectors with Italian options

**Shipping Method Card**:
- Border container with rounded corners
- Radio button selection on left
- Logo placement (Poste Italiane) centered or right-aligned
- Delivery timeframe text: text-sm
- Price aligned right with font-semibold
- Full width, acts as clickable selection area

**Payment Method Card**:
- Similar structure to shipping card
- Radio button + descriptive text
- Icon or logo if applicable
- Padding: p-4

**Order Summary**:

Product Line Items:
- Product image: Small thumbnail (w-16 h-16, rounded)
- Product name and quantity layout: Flex row with space-between
- Quantity badge: Small circular or rectangular indicator
- Individual price: Right-aligned
- Border-bottom separator between items

Price Breakdown:
- Subtotal, Shipping, Total rows
- Label on left, price on right (justify-between)
- Spacing: space-y-3
- Total row: Prominent with larger text and bold weight
- Border-top separator before total

**Primary CTA Button**:
- Full width of container
- Large height: py-4
- Rounded corners: rounded-lg
- Bold, uppercase text
- Icon optional (shopping cart or arrow)
- Fixed to bottom on mobile for easy access

**Trust Badges**:
- Centered below purchase button
- Icon + text combination
- Small size: text-xs to text-sm
- 14-day return policy message
- Minimal padding: py-3

### Form Layout Specifics

**Field Order** (matching screenshot):
1. Phone Number (Numero di telefono)
2. Full Name (Nome e Cognome)
3. Street Address (Indirizzo)
4. City (Città/Paese)
5. Postal Code (CAP) - smaller width
6. Province (Provincia) - dropdown, smaller width
7. CAP and Province on same row (two-column grid)

**Validation States**:
- Error: Red border with error message below (text-sm)
- Success: Subtle indication without breaking flow
- Real-time validation on blur preferred

### Interactive Elements

**Radio Selections**:
- Custom styled radio buttons with checkmark or filled circle
- Entire card clickable, not just radio button
- Selected state: Subtle border or shadow change
- Hover state: Very subtle background shift

**Button States**:
- Default: Solid fill
- Hover: Slight darkening or shadow lift
- Active: Pressed appearance
- Disabled: Reduced opacity with cursor-not-allowed

### Responsive Behavior

**Mobile (< 768px)**:
- Stack all content in single column
- Order summary moves below form OR sticky at bottom
- Full-width inputs and buttons
- Reduced padding: p-4
- Fixed CTA button at bottom of viewport

**Desktop (≥ 768px)**:
- Two-column layout with form left, summary right
- Sticky order summary (position: sticky, top-4)
- Increased padding: p-6 to p-8
- Max-width constraints on very wide screens

### Content Strategy

**Language**: Italian throughout
- Form labels in Italian
- Button text: "Acquista" or "Completa Ordine"
- Trust messaging: "14 giorni per cambiare idea" type messaging
- Error messages in Italian

**Trust Elements**:
- Secure payment indicators
- Return policy prominent
- Shipping guarantee messaging
- SSL/Security badges minimal and tasteful

### Images

**Logo Placement**: Poste Italiane or carrier logo within shipping method card - small, 40-50px height

**Product Thumbnails**: Square ratio, 64x64px, rounded corners, object-cover for consistent sizing

**No Hero Image**: This is a checkout page - no hero section needed, start immediately with form

**Trust Badge Icons**: Small vector icons (16-20px) for security, returns, shipping guarantees

---

**Critical Success Factors**: 
- Form must feel trustworthy and legitimate
- Pricing must be 100% transparent and clear
- Primary action (Purchase button) must be unmissable
- Mobile experience must be optimized for one-handed operation
- Visual hierarchy guides eye from address → shipping → payment → purchase
- Italian language and cultural norms respected throughout
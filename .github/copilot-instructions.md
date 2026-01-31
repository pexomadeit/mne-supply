# MNE Supply - AI Coding Instructions

## Project Overview
MNE Supply is a static e-commerce website for a local retail business in Fargo, ND & Moorhead, MN specializing in premium tech, protective cases, and fragrances. The site emphasizes same-day/next-day local pickup with cash payment options.

## Architecture

### Static Site Structure
- **No backend/database**: All product data is hardcoded in HTML
- **11 HTML pages** with shared navigation and CSS styling
- **Single stylesheet** (`styles.css` - 3,116 lines) with dark theme using CSS variables
- **Client-side cart** using localStorage (not persisted to server)

### Page Organization
- **index.html**: Homepage with featured product and hero sections featuring 3 categories
- **shop.html**: Full product catalog with JavaScript filtering by 3 categories
- **Specialized category pages**: accessories.html, phone-cases.html (Tech & Cases), fragrances.html
- **Info pages**: about.html, contact.html, newsletter.html, login.html, products.html

### Key UI Components
1. **Navigation**: Sticky navbar with dropdown menu (`.navbar`, `.dropdown-menu`)
2. **Product Cards**: Reusable component with emoji icons, title, spec, price, add-to-cart button
3. **Shopping Cart**: Modal overlay system with localStorage persistence
4. **Hero Sections**: Full-width sections with overlay effects and gradient backgrounds

## JavaScript Patterns

### Cart System (Duplicated across pages)
Each HTML file contains its own copy of identical cart logic:
```javascript
- loadCart() / saveCart() - localStorage management (localStorage.getItem/setItem 'cart')
- addToCart(productName, price) - pushes to cart array
- removeFromCart() / updateQuantity() - modifies cart items
- updateCartDisplay() - renders modal with running total calculation
- proceedToCheckout() - redirects to login.html
```
**Note**: This cart system is purely client-side and resets on page refresh.

### Product Filtering
- Used in `shop.html` and `products.html`
- `filterProducts(category)` shows/hides product-category divs with display property
- Categories: 'all', 'accessories', 'cases', 'fragrance' (clothing removed)

### Initialization Events
- `DOMContentLoaded` calls `updateCartCount()` to populate 🛒 badge
- `window.onclick` closes cart modal when clicking outside

## CSS Architecture

### Design System
**Color Variables** in `:root`:
- Primary: `#0a0e27` (dark navy)
- Secondary: `#1a1f3a` (dark blue)
- Accent colors: blue (`#3b82f6`), purple (`#8b5cf6`), cyan (`#06b6d4`)
- Light: `#f8fafc`, white: `#ffffff`
- Gray tones for text

**Typography**:
- Primary font: 'Inter' (sans-serif)
- Display font: 'Poppins' (headings)
- Default line-height: 1.6

**Spacing & Effects**:
- Border-radius: 12px
- Transition: `all 0.3s cubic-bezier(0.4, 0, 0.2, 1)`
- Heavy use of gradients and box-shadows for depth

### Layout Patterns
- `.container`: Fixed-width wrapper with horizontal padding
- `.product-grid`: CSS Grid (typically 4 columns, responsive)
- `.hero-overlay`: Dark opacity div over background images
- Flexbox used for navigation and card interiors
- Fixed navbar with `position: sticky; top: 0; z-index: 100`

### Responsive Strategy
- Mobile breakpoint: `@media (max-width: 768px)`
- Tablet breakpoint: `@media (max-width: 1024px)`
- Cards and grids collapse from 4 → 2 → 1 column
- Cart modal becomes full-width on mobile

## Development Workflow

### Adding Products
1. Add `<div class="product-card">` with data-category attribute in category section
2. Include: `.product-image` (emoji), `<h4>` title, `.product-spec`, `.product-price`, `<button>` with `onclick="addToCart('Name', price)"`
3. Update any product filter functions if new categories added

### Updating Navigation
- Edit `.nav-menu` in navbar
- Ensure matching links across all pages (they're duplicated, not templated)
- Keep active state with `class="active"` on current page link

### Styling Changes
- Modify CSS variables in `:root` for global color changes
- Component classes follow pattern: `.{feature}-{element}` (e.g., `.product-card`, `.cart-modal`)
- Maintain responsive breakpoints at 768px and 1024px

## Common Issues & Conventions

### Known Patterns
- **Code duplication**: Cart JavaScript is copy-pasted across all pages (no shared script file)
- **No build process**: Static files served directly
- **LocalStorage key**: Cart uses 'cart' as localStorage key - changing this breaks persistence
- **Product images**: Using emoji placeholders instead of real image assets
- **SEO optimized**: Each page has unique meta tags (Open Graph, Twitter Card, description)
- **3 categories only**: Tech/Accessories, Cases, and Fragrances - clothing removed in 2026

### Maintenance Notes
- When modifying cart logic, update ALL occurrences (index.html, shop.html, phone-cases.html, products.html, plus category pages)
- CSS selectors are global - check all pages before renaming classes
- The same navigation menu appears on all pages with slight variation per active link

## Performance & Accessibility Notes
- CSS uses backdrop-filter blur effects (may not work in older browsers)
- Cart notification animation uses CSS transitions, not JavaScript
- All buttons use semantic `<button>` elements
- Images are emojis (maximum performance, no alt text needed)
